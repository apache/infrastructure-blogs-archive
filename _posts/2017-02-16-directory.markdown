---
layout: post
title: Apache Mavibot history
date: '2017-02-16T00:00:00+00:00'
categories: directory
---
<h1>Introduction</h1>
<p>
First of all, let me introduce <b>Apache Mavibot</b>: it's a <a href="https://en.wikipedia.org/wiki/Multiversion_concurrency_control">MVCC</a> <a href="https://en.wikipedia.org/wiki/B%2B_tree">B+
tree</a> library in Java under an <a href="https://www.apache.org/licenses/LICENSE-2.0">AL 2.0 license</a> (<b>MVCC</b> stands for
<em>Multi-Version Concurrency Control</em>). The whole idea is to have a B-tree
implementation that never crashes, and does not use locks to protect the
data against concurrent access (well … while reading).
</p>
<p>
The B+ tree is a variant of a B-tree, where values are only stored in
the leaves, not in internal nodes.
</p>
<p>
Ok. Good. You don't know much about Mavibot after this introduction, so
I'll dig a bit deeper in this post. Let's start with the original idea.
</p>

<h2>Apache Directory, CouchDB, and some other databases...</h2>

<p>
Back in 2009, I was attending the <a href="http://archive.apachecon.com/c/acus2009/">Apache Conference in Oakland</a>. I had been
working on the <a href="http://directory.apache.org">Apache Directory project</a> for a bit more than a 4 years
and a half. <em>Apache Directory</em> is a <b>LDAP</b> server written in Java, and we
chose to store data in B-trees. There was a very limited choice back
then, and the library we used - and still use as of today - was <a href="http://jdbm.sourceforge.net/">JDBM
</a>, a java avatar of <a href="http://www.gnu.org.ua/software/gdbm/">GDBM</a>.
</p>
<p>
<b>JDBM</b> is written in Java, implements <b>B+trees</b> and has transactions support
(experimental), but it has one big drawback: it's not a cross-B-trees
transaction system. And that does not fit our requirement in <b>LDAP</b>.
</p>
<p>
An alternative could have been <b>Berkeley DB &tm;</b>, which released a Java
edition of its database, but its license was incompatible with the AL
2.0 license. Moreover Berkeley DB was bought by <b>Oracle</b> in 2006, so it was
simply not an option.
</p>

<h2>What’s wrong with using JDBM?</h2>
<p>
In <b>LDAP</b>, an update operation impacts one single entry but this entry
uses many <em>AttributeTypes</em>, which can be indexed. In other words, an
update will impact as many B-trees as we have indexes (and a bit more).
In order to guarantee that an entry UPDATE is consistent, we must be
sure that either all or none of the indexes have been flushed to disk:
otherwise we might end with an inconsistent database, where some indexes
are up to date when some other aren't.
</p>


<a href="https://blogs.apache.org/directory/mediaresource/847a2aa6-dce8-4d6f-9a65-79c74219d785"><img src="https://blogs.apache.org/directory/mediaresource/847a2aa6-dce8-4d6f-9a65-79c74219d785" alt="indexeldap.png" height="50%" width="50%"></img></a>

<p>
Even worse, in the event of a crash, we might simply not be able
to restart the server because the database gets corrupted (and
sadly, we are experiencing this problem today...).
</p>
<p>
So in <b>Oakland</b>, I went to the <a href="http://couchdb.apache.org/">Apache Couch-DB</a> presentation (sadly,
the slides are not anymore available), and was struck by the idea behind
their database: <b>MVCC</b>. Crucially when you start to use the
database at a given revision, you always see everything associated with
this revision, up to the point you are done. Sounds like <b>Git</b> or
<b>Subversion</b> … actually, it's pretty much the same mechanism.
</p>
<p>
Being able to process some read operations on a specific version of the
database guarantees that no update will ever corrupt the data being
processed. And every time we want to access the database, the very first
thing it will do is to select the latest available version: this is
all we will see during the operation processing. Perfect when you don't really care about having a fresh view of the stored data at any time, which is the case in <b>LDAP</b>.
</p>
<p>
But <b>Apache CouchDB</b> was written in <b>Erlang</b> :/ Anyway, the discussion we
had with the Directory team was really about moving to a <b>MVCC</b> database.
</p>

<h2>Transactions</h2>
<p>
Transactions are another big missing feature in <b>LDAP</b>. This is not something that was
in the air back then: it was specified only <a href="https://tools.ietf.org/html/rfc5805">one year later</a>. Of course, the original specifications said that every operation is atomic, but there is no requirement for multiple operations to be atomic (and we often need to update two entries in <b>LDAP</b>, and to guarantee that those two operations are either completed, or
roll-backed). Think about user/group management...
</p>
<p>
<b>Alex Karasulu</b> always had in mind that we needed a transactional database
in Apache Directory, too. And his point was proved correct when years
later, we faced the first database corruptions. It's a bit sad that we
ignored this aspect for so long :/
</p>
<p>
Anyway, we needed (a) transactions and (b) a rock solid database that could
resist any type of crash.
</p>

<h2>Locks</h2>
<p>
For some time, we tried to mitigate the consistency problems we had by
adding tons of locks. As we weren't able to protect the database against
concurrent reads and writes we made them exclusive (i.e.
when some write is processed, no read can be processed). This was
slightly better, but it came at a huge cost: a major slowdown when
writes were done. Also it was not good enough: long-lasting searches
were just killing us, as there were no solution to guarantee that an
entry for which we had a reference would still be present in the database
when we needed to fetch it. In such cases, we simply ended up by
discarding the entry.  Last, not least, a crash in the middle of an
update operation would leave the database in a potential inconsistent
state, which would make it impossible to start again (this was somehow
mitigated by adding a 'repair' mode lately, but this is just an horrible
hack).
</p>

<h2>Mavibot first steps</h2>
<p>
So we needed something better, which turned out to be <b>Mavibot</b>. We started working
on Mavibot in June 2012 (<a href="http://svn.apache.org/viewvc/labs/mavibot/README.txt?revision=1349589&view=markup&pathrev=1349589">Jun 13 00:04:10 2012, exactly</a>).
</p>
<p>
The funny thing is that <b>OpenLDAP</b> started to work on the exact same kind
of database 1 year before (<a href="http://www.openldap.org/devel/gitweb.cgi?p=openldap.git;a=commit;h=d620d4368a7ee17d60f2b381d4c80b22c68ba8e2">LMDB</a>) - even if the <a href="http://marc.info/?l=openldap-devel&m=124253456912512">discussion about the
need for such a database started in 2009</a>. Parallel discussions,
parallel developments, we have always shared a lot!
</p>
<p>
The very first released version of <b>Mavibot</b> was out one year later, in
June 2013, followed by 7 other versions (all of them milestones). At
some point, we added a <b>MVBT</b> partition in <b>ApacheDS</b>, in 2.0.0-M13 (and it
was using a SNAPSHOT!!! Mavibot 1.0.0-M1 was used in <b>ApacheDS</b>
2.0.0-M15). This was 'good enough' to have the <b>LDAP</b< server working (and
it was 2 to 5 times faster than <b>JDBM</b>, too ;-), but it didn't offer all
we wanted to add: typically, we didn't have transaction support.
</p>
<p>
So why isn’t <b>Mavibot</b> the <b>Apache Directory Server</b> backend of choice today?
</p>
<p>
Well, we don't have cross B-tree transactions, so we are pretty much in
the same situation as with <b>JDBM</b> (except that it's faster, and we also
have a bulk-loader for <b>Mavibot</b>). Adding cross-B-trees transaction is not
a piece of cake, and it requires some thinking. Sadly, it arrived at a
moment where the team had less time to work on it (new jobs, family,
you name it).
</p>
<p>
So in 2017, the effort has been rebooted, and we do expect to have a
working version soon enough!
</p>
<p>
I'll blog later on about various technical aspects on <b>Apache Mavibot</b>, so keep tuned !
</p>
