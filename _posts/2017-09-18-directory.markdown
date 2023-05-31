---
layout: post
title: Mavibot Transactions
date: '2017-09-18T00:00:00+00:00'
categories: directory
---
So here is the blog post I promised 6 months ago... It took a bit of a time, due to a family expansion that sucked up pretty much all our free time :-)

Let's start stating the obvious !

<h3>What is a transaction ?</h3>


It's simply 'a unit of work'. That means it's either fully completed, or it never happened. This is critical to database, as it makes certain the database is <strong>always</strong> in a consistent state. But it's more that that. A Transaction system must provide the following properties (aka ACID) :

<h4>Atomicity</h4>

You can't split any of the tasks in a transaction. The transaction is always seen as a whole.

<h4>Consistency</h4>

The database must be valid after the transaction as it was before. It's mainly about guarantying that implied updates (like triggers, constraints, etc) are done within the transaction

<h4>Isolation</h4>

Here, we are going to use a restricted version of 'isolation' : read committed. That means we guarantee a user will always be able to read consistent data at any time, and will not see 'dirty' data (ie, data being written during a transaction). However, when the transaction is written, then a user doing a second read may not see the same data he got with a previous read. This has some deep impact on the way the user should manage the results : there is <strong>no</strong> guarantee that a data one get with a read is still valid a few seconds later. We don't lock the database on reads.

<h4>Durability</h4>

This seems trivial : once committed, the transaction will let the database in a stable state, for ever. It's mainly about guaranteeing that the database survives a crash. 

<h3>Mavibot transaction implementation</h3>

So far, we can assume that being a <strong>MVCC</strong> system, <strong>Mavibot</strong> already covers all those aspects :
<ul>
  <li>Atomicity : Updates are either done or not, in any case, a new version is not seen until it's fully committed. </li>
  <li>Consistency : You can't modify some existing version. An update does not change anything i the database, it just adds a new version</li>
  <li>Isolation : A reader just sees a single version</li>
  <li>Durability : Once a new version is added, it's visible by readers using it until the readers don't need anymore the data.</li>
</ul>

The only part we have to take care of is the way we make a new version available to readers : at some point, every new reader should use the new version. In order to make that possible, we just have to switch one single reference to the current version.

<h4>Mavibot B-trees</h4>

At this point, I have to come back to how <em>B-trees</em> are managed in <strong>Mavibot</strong>.
<p>
A <em>B-tree</em> consist of three separate elements :
<ul>
  <li><em>B-tree</em> info : this data structure stores all the characteristics of the <em>B-tree</em> (it's name, the serializers being used, etc). It's immutable.</li>
  <li><em>B-tree</em> header : the starting point from which the <em>B-tree</em> can be pulled from the disk. It points to the root page, and has some extra information about the <em>B-tree</em> (number of elements, revision, Page ID, etc)</li>
  <li><em>B-tree</em> root page : the top level page, this is where data are stored (and in all the children nodes and leaves)</li>
</ul>

Basically, we always use the <em>B-tree</em> header as a reference when reading or updating a <em>B-tree</em>.
<p>
Now, we have special <em>B-tree</em>s that are used to manage <strong>Mavibot</strong> :
<ul>
  <li><em>B-tree</em> of <em>B-tree</em>s (aka <strong>BoB</strong>) : This special <em>B-tree</em> contains a reference to all the existing <em>B-tree</em>s in <strong>Mavibot</strong>. It's critical as we need to know about the managed <em>B-tree</em>s when we start <strong>Mavibot</strong>. Every <em>B-tree</em> update will result in this <em>B-tree</em> to be updated.</li>
  <li>Copied Pages <em>B-tree</em> (aka <strong>CPB</strong>) : This other special <em>B-tree</em> stores the list of pages ID that has been copied when a new update is injected. This is used by the Pages Reclaimer (which is a process that move old pages into the free-page list. Think of it as a Garbage Collector, because this is exactly what it is). It's updated after each update.</li>
  <li>Free-Page list : it's a chained list of Pages that can be reused. The Pages Reclaimer will update this list, and the writer will pull pages from this list before fetching some new pages at the end of the file if there is no more free pages.</li>
</ul>

I won't go any further here, I just wanted to expose those management elements for a better understanding of what will follow.

<h3>So what's the point of transaction in Mavibot?</h3>

Well, we could live without them, but it would be very inefficient. There are two problems :
<ul>
  <li>performance : Flushing the modified pages every time we update a <em>B-tree</em> means we also flush the management <em>B-tree</em>s as many times.</li>
  <li>cross <em>B-tree</em> transaction : This is the critical piece. Users may need to ensure <strong>ACID</strong> properties across multiple <em>B-tree</em>s.</li>
</ul>

This is where adding a higher level of transaction is required and good to have. Let's see what that means more in detail.

<h4>Performance</h4>

The following two schema show what happens when we update one single tree 5 times (from the very beginning, with an empty <em>B-tree</em>, up to a point we have added 4 elements in it.
<p>
For the purpose of this demonstration, I'm using pages that can contain only 2 elements, but we would get the exact same result with bigger pages (except that it would be way more dreadful to draw :-)
<p>
In this schema, each page has a reference (its offset on disk), noted O<x> where 'x' is a integer number.
<p>

<img src="https://blogs.apache.org/directory/mediaresource/5a0965c0-ae29-4d52-abe4-82fd65bc4ad2" alt="one-op-per-txn.png"></img>
<p>
<p>
Pretty complicated...
<p>
What if we gather updates within a single transaction ?
<p>
Here is what we get if 3 of the previous updates are done within one single transaction :
<p>
<p>

<img src="https://blogs.apache.org/directory/mediaresource/fda50f5a-f9b8-429e-ab67-af66cdaa083c" alt="N-op-per-txn.png"></img>
<p>
As we can see, we have way less pages being created, mainly in the <strong>BoB</strong> and <strong>CPB</strong> <em>B-tree</em>s. For the record, we avoid flushing 15 pages (out of 36), a 40% improvement.

<h4>Cross-B-tree transaction</h4>

Is it realistic to gather many operations on many <em>B-tree</em>s into a single transaction ? It feels like we may lose some data if the system crashes in the middle of a transaction...
<p>
Actually, this is badly needed : it's quite rare that we update only one <em>B-tree</em>. Typically, when using <strong>Mavibot</strong> in <strong>ApacheDS</strong>, we want to commit a transaction only when the full LDAP update is completed, and that involves many <em>B-tree</em>s :
<ul>
  <li>Master table</li>
  <li>Rdn table (potentially many times, as many as we have RDNs in the DN)</li>
  <li>Present index</li>
  <li>ObjectClass index</li>
  <li>entryCSN index</li>
  <li>Alias index</li>
  <li>One-Alias index</li>
  <li>Sub-Alias index</li>
  <li>AdministrativeRole index</li>
  <li>Add all the user defined indexes...</li>
</ul>

We do want all these index to be updated all at once, otherwise if we have a crash when only a sub-set of them are cmmitted on disk, and teh rest aren't, then we end up with an inconsistent database.
<p>
So we want a transaction that covers all those <em>B-tree</em>s, and the side benefit is that if any of those <em>B-tree</em>s get updated more than once (like the RDN index), we save some disk access. We also save a lot of disk access as the <strong>BoB</strong> and <strong>CPB</strong> B-tress get updated at the very end, saving some more disk access.

<h4>Side benefits</h4>

We can move forward, and let the user decide that transaction should be committed only after a certain amount of updates being applied. This is typically something you want when pushing a lot of modifications in a batch.
<p>
The drawback is that you may lose  everything from the point you started the transaction if your system crashes at some point before the commit, and it eats some memory as we have to keep the pending modification in memory until the moment the transaction is committed.
<p>
It's a balance : speed vs safety.

<h3>How it all works</h3>

As soon as we start a transaction, we create a copy of the existing <em>B-tree</em>s (well, not all of them, just the parts that are to be protected !) so that the existing readers aren't impacted by the on going modifications.
<p>
Now, for every modification done in a <em>B-tree</em>, we will copy the modified pages in memory, and update them. If a page has already been copied - and is in memory -, we simply reuse the copy. We have a map of all the in-memory pages for that purpose, and every new copy is put into this map.
<p>
When we are done with the modifications, we just have to flush all the pages in this map on the disk : we are done.
<p>
Well, not completely... We also have to update the <strong>BoB</strong> and the <strong>CPB</strong> <em>B-tree</em>s, as usual. The point is that those updates can be done at the very end. Last, not least, we can move  the unused pages into the free-pages list.
<p>
That is the theory, the implementation is a bit complex, and I won't expose it here...
<p>
At this point, a bit of code sample could help :
<p>
<code>
&nbsp;&nbsp;&nbsp;&nbsp;// Create the Mavibot database<br/>
&nbsp;&nbsp;&nbsp;&nbsp;RecordManager recordManager = new RecordManager( "MyData.bin" );<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;// Create a B-tree : we need to start a transaction...<br/>
&nbsp;&nbsp;&nbsp;&nbsp;try ( WriteTransaction transaction = recordManager.beginWriteTransaction() )<br/>
&nbsp;&nbsp;&nbsp;&nbsp;{<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;btree = recordManager.addBTree( transaction, "test", LongSerializer.INSTANCE, StringSerializer.INSTANCE, true );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;}<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;// Add some data in the created B-tree.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;try ( WriteTransaction writeTxn = recordManager.beginWriteTransaction() )<br/>
&nbsp;&nbsp;&nbsp;&nbsp;{<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BTree<Long, String> btree = writeTxn.getBTree( "test" );<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;btree.insert( writeTxn, 1L, "1" );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;btree.insert( writeTxn, 4L, "4" );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;btree.insert( writeTxn, 2L, "2" );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;}<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;// Now, read the B-tree<br/>
&nbsp;&nbsp;&nbsp;&nbsp;try ( Transaction readTxn = recordManager.beginReadTransaction() )<br/>
&nbsp;&nbsp;&nbsp;&nbsp;{<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BTree<Long, String> btree = readTxn.getBTree( "test" );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TupleCursor<Long, String> cursor = btree.browse( readTxn );<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while ( cursor.hasNext() )<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;System.out.println( cursor.next() );<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>
&nbsp;&nbsp;&nbsp;&nbsp;}<br/>
...<br/>
</code>
<p>
This will print "&lt;1,1&gt;", "&lt;2,2&gt;" and "&lt;4,4&gt;".
<p>
A few remarks :
<ul>
  <li>Transactions are implementing <strong>Closeable</strong>, which makes it possible to use them in <strong>try-with-resource</strong> statements. The automatically called <em>close()</em> method will call the transaction <em>commit()</em> method</li>
  <li>W can see we have two transaction flavors : read and write. The Read transaction just get a context in which it applies, and this context is simply the latest committed version. The idea is that the reads won't be impacted by any forthcoming updates, as it works in its own context.</li>
  <li>In any case, you have to pull the <em>B-tree</em> you want to update or read from the <em>RecordManager</em>, which manages the whole system</li>
</ul>

<h3>Conclusion</h3>

This is just a 10 feet high description of the transaction system we use in <strong>Mavibot</strong>, it does not go into details, nor it gives all the keys to be able to use <strong>Mavibot</strong>. The whole idea is to give a sense of what we are working on, and why we need transactions in <strong>Mavibot</strong>.
<p>
We may changes a thing here and there, typically some method names (so don't expect the code to be perfectly representative of what will be the released version). Anyway, it's going to be pretty close !
<p>

In the next blog post, I'll talk more in details about the management <em>B-tree</em>s (<strong>BoB</strong>, <strong>CPB</strong>) and the free-pages management (ie, the Pages Reclaimer). There are quite interesting aspects to discuss when it comes to reclaiming pages :-)
