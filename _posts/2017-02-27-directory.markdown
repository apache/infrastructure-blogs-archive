---
layout: post
title: Let's meet B-trees
date: '2017-02-27T00:00:00+00:00'
categories: directory
---
<h1>Render unto Caesar</h1>

<b>B-trees</b> have been invented in the 70's by <b>Rudolf Bayer</b> and <b>Edward McCreight</b>. They were both working at <b>Boeing &trade;</b> and wanted to design a better version of <b>Binary trees</b>, when addressing loads of data stored in disks. The <b>B</b> in <b>B-tree</b> comes from an aggregation of the three <b>B</b>s in <b>Bayer</b>, <b>Boeing</b> and <b>Binary-tree</b>.
<p>
They wanted to improve browsing efficiency. The idea was to store values sequentially on the disk so as to limit the number of seek operations. Aggregating consecutive values in pages so that a single read returns a few of them is faster than fetching them one by one in different places on a disk.

<h1>Definition</h1>
What is a <b>B-tree</b>?

Let's first define some of the terminology in use:

<ul>
<li><b>Page</b>: either a <b>Node</b> or a <b>Leaf</b></li>
<li><b>Node</b>: a <b>Node</b> is a page in a <b>B-tree</b> that will have children</li>
<li><b>Leaf</b>:  a <b>Leaf</b> is a page in a <b>B-tree</b> that will not have child</li>
<li><b>Root page</b>: this is the top level page. It can be a <b>Node</b> - and in this case, the <b>B-tree</b> has many levels - or it can be a <b>Leaf</b> - and in this case all the values are hold int this leaf.</li>
</ul>

And also a few properties/constraints of a <b>B-tree</b>:

<ul>
<li>A <b>Page</b> can contain up to N values</li>
<li>A <b>B-tree</b> page always contain at least N/2 values</li>
<li>The only page that can contain less than N/2 values is the root page, that can contain zero values (which mean the <b>B-tree</b> is empty), or 1 to N values</li>
</ul>

Let's continue with the definition: it's a <a href="https://en.wikipedia.org/wiki/B-tree">self-balancing tree data structure that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time</a>. Thanks Wikipedia :-)

<ul>
<li>Self-balancing: that means the height of the tree is automatically adjusted on each update to be evenly balanced. It allows accessing any data in a equal number of operations (well, 'equal' has to be taken with caution: we are talking about the number of nodes we have to read before fetching the data). For instance, if you have a <b>B-tree</b> that has 5 levels of <b>Nodes</b>, then fetching an element from this <b>B-tree</b> will 'cost' 5 node reads.</li>
<li>Sorted: the data are stored in a specific order, the lowest value being on the left of the <b>B-tree</b> and the highest value on the right of the <b>B-tree</b>. That also means you have to provide a comparator in order to be able to sort those values.</li>
<li>Search: it's the operation of going down the <b>B-tree</b> to find a specific value. There are various possibilities here but you always start from the top of the <b>B-tree</b> and go down the <b>Nodes</b> to the value you are looking for.</li>
<li>Sequential access: this is where it starts to be interesting. A <b>B-tree</b> allows you to browse the values, and to get them one by one in a very efficient way. Since those values are sorted, fetching the next value is a very simple operation. Typically, this is something you can't do with a <b>Map</b></li>
<li>Insertions and deletions: It's always possible to add or remove values from a <b>B-tree</b>. This will potentially reorganize the <b>B-tree</b> structure, but usually not that much. Most of the time, one single <b>Leaf</b> will be modified, but sometimes many <b>Nodes</b> will be modified up to the root <b>Node</b>.</li>
<li>In logarithmic time: That is the key. In big O notation, it reads like <b>O(logn)</b>. For instance, if each <b>Node</b>/<b>Leaf </b>holds 16 values, that means a <b>B-tree</b> storing 1 000 000 values will have a height of 7 maximum and 5 minimum (it depends how full are the <b>Nodes</b>/<b>Leaves</b>). If each <b>Node</b>/<b>Leaf</b> contain 8 values each, a 7 level <b>B-tree</b> can hold up to 8^7 values, ie 2,097,152 values. If each <b>Node</b>/<b>Leaf</b> contains 16 values, a 5 level <b>B-tree</b> can hold up to 16^5 values, ie 1,048,576 values. If we expect each <b>Node</b>/<b>Leaves</b> to be 2/3rd full, ie containing 10 values, then a 6 level <b>B-tree</b> can hold 1 000 000 values (10^6). That is pure math, nothing new under the sun!</li>
</ul>

That pretty much defines what a <b>B-tree</b> is.

<h1>Why have B-trees been invented?</h1>

So the whole idea was to speed up operations made on a binary-tree. In a binary-tree, values are stored so that each value has a left and right child. Searching in a binary tree is all about traversing nodes, down to the value you are looking for. The biggest issue is that binary-trees aren't necessary balanced: for any set of values, you may have any kind of distribution, from perfectly balanced binary-trees where at any point of the tree, the height of the <b>B-tree</b> is equal to <b>log2(N)</b>, to a degenerated <b>B-tree</b> where you have N levels.
<p>
Actually, binary trees are faster because the number of comparisons you have to make to retrieve a value is also equal to the number of levels. If the binary-tree is balanced, this will be l<b>og2(nb values)</b>. In a <b>B-tree</b>, as you hold up to N values in each <b>Page</b>, the number of pages you will fetch is <b>logN(nb values)</b>, but for each fetched page, you also have to do <b>logN(nb values in the page)</b> comparisons per level. So if N is 16, with 1 000 000 values stored, and each page 2/3rd full on average, you will do <b>(nb levels) * log2( nb value per page)</b> comparison. That is 6 * 4 comparison, ie 24 - where 4 stands for <b>log2(10 values per page on average )</b> - instead of 20 (which is <b>log2(1000000)</b>).
<p>
Ok, so <b>B-tree</b> are less efficient than balanced binary-trees, that's a fact. Why then should we use a <b>B-tree</b>? Simply because we don't process data in memory only: we usually fetch them from disk. And that is the key: fetching something from a disk is a costly operation (even if you are using a <b>SSD</b>). Back when <b>B-trees</b> were invented, data were read from a sequential support: a band or a magnetic disk. The probability that we may fetch many consecutive values was extremely high, thus storing many values per page was a great idea. 

<h1>Mavibot and ApacheDS</h1>

<b>Mavibot</b> was designed as a replacement for <b>JDBM</b> for the reason I explained in my previous post. Let's now see what are its requirements.
<p>
We needed some <b>B-tree</b> implementation in Java, under an <b>AL 2.0</b> license, with support of cross-B-tree transaction. The solution was to implement a <b>Multiversion concurrency control</b> model, aka <b>MVCC</b>. What are the characteristics of such a system ?

<ul>
  <li>No locks: no need for those since reads are working on immutable data, and we have only one single writer, which creates a new version;</li>
  <li>Consistency: since data are immutable once written, a reader will always have access to them;</li>
  <li>Crash resistance: As the written data are immutable, they will always be available in their initial state. A new version will be visible only when it has been completely written on disk, or will not be visible. In case of a hard crash, the system will start with the latest visible version, thus in a stable state;</li>
  <li>Immediate availability on restart: In the event of a crash, readers will always see the latest version, regardless of a potential updates that could have been interrupted by the crash. In the worst case scenario, data being updated will be lost and the associated pages will remain unreachable;</li>
</ul>

Those characteristics come at a cost:

<ul>
  <li>One single writer: as we want cross-B-tree transactions, that will limit seriously the update throughput;</li>
  <li>Versions visibility: a version will remain visible until no reader uses it. This can be for quite a long time, so we may have to keep a lot of data on disk;</li>
  <li>Disk space: the simple fact we keep many versions alive will consume more disk space than it would in a normal system;</li>
  <li>Data relevance: since a reader is stuck with a given version, it cannot work on a updated version unless it starts a new read;</li>
  <li>Complex cleanup: In order to discard 'dead data' (ie data for version not anymore in use), we need to implement a system known as a Garbage Collector, which can be complex and costly;</li>
</ul>

The third point (disk space) is important. In a <b>B-tree</b>, for any update, we have to copy each <b>Node</b> we are traversing to go to the leaf containing the data, and also copy this leaf. For a <b>B-tree</b> containing 1 million elements with pages holding up to 256 elements, any update will require 3 levels - 2 <b>Nodes</b> and a <b>Leaf</b> - to be copied. That means for 1000 updates, we will eat <b>3*(page size)*1000</b> bytes, ie 12Mb for 4096 byte pages. Even worse, we also have to keep some side data which will double this size. With a standard <b>B-tree</b>, we will just eat what is needed to hold those updates in the <b>B-tree</b>, ie something like a few Kb.
<p>
Now, if we keep all the versions, we might end up with a really huge database. By the way, this is what happens when you use <b>git</b>, except that <b>git</b> does not copy intermediate <b>Nodes</b> :-)
<p>
As I said, we needed <b>Mavibot</b> for <b>Apache Directory Server</b>, for which those drawbacks weren't critical: <b>LDAP</b> favors reads over writes, so having only one writer is a non issue. Data relevance is not a big issue: just because a phone number does not change that frequently, it's not really a problem that the number a reader has grabbed is not relevant anymore when the reader wants to use it. In the worst case scenario the reader will ask it again, and get the new one back. Now, for passwords, it might sounds a bit more critical, but actually, the time frame during which a password is read and used is really short, so when you change it, it's very unlikely to impact any reader. If it does, then it's not really a problem, because any logged application has been authenticated with the old password, and the administrator should have logged out anyone *before* allowing the password to be changed.
<p>
That being said, the most important feature for <b>ApacheDS</b> is consistency, which <b>Mavibot</b> provides. If you are to write a bank application, then this is not the right database for you...

<h1>How does it work ?</h1>

The <b>B-tree</b> we use in such a system is a bit specific. This is also a specific flavor of B-tree, because we decided not to store data in the Nodes.
<p>
Typically, we also cannot chain leaves together (in some <b>B-tree</b> implementations, chaining leaves increases browsing speed - because there is no need to go up to the parent <b>Nodes</b> to fetch the next values -. It also limits the number of locks needed to update the <b>B-tree</b>).

<h2>Updates</h2>

Let's see what's going on when we inject some values in a <b>B-tree</b>. Here, we will insert <em><b>{E, A, D, I, K}</b></em> in that order. The very first version will be:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/8ac770eb-1613-49dd-bef9-e3c587a9c53e" alt="rev1.png"/>
<p>
Here, the root page is a <b>Leaf</b>, and contains one value, <em><b>E</b></em>. Then we insert the value <em></b><b>A</b></em>, which is added to the root page, and we create a copy of this root page for that purpose:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/f87ae4c8-1289-4866-b345-4fea54762695" alt="rev2.png"/>
<p>
The insertion of the third value, <em><b>D</b></em>, can't be done in the root page, because it's already full. We have to create a parent <b>Node</b> which refers to two leaves, containing the three values:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/1d8eebea-12fe-4a0b-afd5-378bd9d465ce"/>
<p>
As you can see, we have created two leaves and copied the root-page. Let's insert a fourth value, <em><b>I</b></em>:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/f6cd7d6e-c535-48bf-ac59-ce34e8f023cb"/>

Each new addition will copy some of the pages from the previous versions, but may keep some references to previously created pages. Here, version 4 has split the right leaf into 2 new pages, but we have kept the left leaf intact. The parent node is also copied. Now, a reader using version 3 will still see only three values (<em>A</em>, <em>D</em> and <em><b>E</b></em>) while a new reader will see 4 values ( <em><b>A</b></em>, <em><b>D</b></em>, <em><b>E</b></em> and <em><b>I</b></em>). In the process we have created 3 new pages.
<p>
Insertion is actually done bottom-up: we add the new value in the leaf, and if the leaf is already full, we create a new leaf, spread the data between the 2 leaves, and go back to the parent to add a new reference to the created leaves. Of course, this has to propagate up to the root page, and if the current root page is full, then we have to split it again. This is what happens when we insert a fifth value (<em><b>K</b></em>) in the previous <b>B-tree</b>:<br/>
<p>
<img src="https://blogs.apache.org/directory/mediaresource/e96c0956-ba5a-4dcb-9f3c-d59531d04ad9" alt="rev5.png"/>
<p>
This time, the <b>B-tree</b> has 3 levels. Version 5 would look like this to any new reader:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/e3a67f33-5ebe-4e1a-ad87-93607d81c844" alt="rev5-alone.png"/>
<p>
It's goes on and on like this. In real life, we won't store only 2 values in each leaf: that would be way too expensive. But it's easier when drawing what happens :-)
<p>
At some point in time, when no reader is pointing to a version, we can reclaim the unused pages (to be explained in another blog post).
<p>
I haven't talked about deletion. Let's see what happens when we delete the value <em><b>E</b></em>, for instance:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/3a3d2956-62ca-44c9-ab86-33c0a8627890" alt="rev6.png"/>
<p>
This time, we have just created a new root page, pointing to existing previous pages. Note that you can still browse the tree and get all the data for this version: <em><b>{A, D, I, K}</b></em>.
<p>
One side note: you can see that if we were to link leaves to each other creating a new version would leads us to copy the whole <b>B-tree</b>, not only new or modified pages.


<h2>Browsing</h2>

The most frequent operation is browsing. We may want to find a specific value or to read many values, starting from a specific part of the <b>B-tree</b>. Typically, if the values are dates, then reading all the values after a given date is a matter of finding the starting date, and move on toward the end of the <b>B-tree</b>.
<p>
However it gets a bit more complicated in the absence of links between leaves.

<h3>Fetching a value</h3>

First let's see what happens when we try to fetch a value. We always start from the root Page. Here, let's say we want to retrieve the value <em><b>I</b></em> from version 5. <em><b>I</b></em> is not present in the root page, so we will go down the tree on the right side. The <em><b>I</b></em> key is present in the node at the second level, so we move again down to the right page, which is a leaf, and we can now get the data.

<p>
The rules are simple:

<ul>
  <li>When in a <b>Node</b>, if the key is found, then go down to the right page</li>
  <li>If the key is not found, go to the page on the left of the key immediately above the key, except if the key is higher than the highest present key: in this case, go the the right page of the latest present key</li>
  <li>When the page is a leaf, if the key is not present, then we have no such key in the <b>B-tree</b></li>
</ul>

The following image shows how we walk the tree to get the value we are looking for:

<img src="https://blogs.apache.org/directory/mediaresource/2527c339-9591-4fb0-a00d-e5963353436c" alt="fetch-i.png"/>

<h3>Browsing many values</h3>

Now, we can decide to browse the <b>B-tree</b> values, either from the beginning or from a given value. This is slightly more complex, because we can't easily move from one leaf to another: we have to go up to the parents to do that. The following image shows such a browsing path:
<p>
<img src="https://blogs.apache.org/directory/mediaresource/2c1fe3f1-8cec-405a-b298-33b255e98df1" alt="browse-all.png"/>
<p>
As we can see, we start from the bottom-left, get back to the parent, and down to the second leaf, back to the root page and down to the leaf containing <em><b>E</b></em>, up to the parent and down to the last leaf. Browsing is not a simple operation !
<p>
One important thing to note here though is that every reader will start from the root-page, and that the intermediary nodes are accessed many times. That gives us a clue about what pages need to stay in memory, but that will be discussed later on.

<h1>Conclusion</h1>

We are done with this blog post, which detailed the basic operations on a <b>MVCC</b> <b>B-tree</b>. In the next post, I will explore transactions.
