---
layout: post
title: Apache Lucene.Net 2.9.4g Incubating released
date: '2012-02-14T00:00:00+00:00'
categories: lucenenet
---
<p>It took about two months to fully roll out the 2.9.4g branch out the door. This release mostly replaces the plumbing of 2.9.4 with the .NET generic classes. One of the many benefits is the ability to use more .NET like code such as foreach (instead of GetEnumerator/MoveNext). There are a couple of API changes to be aware of:</p> 
  <ul> 
    <li>StopAnalyzer(List<string> stopWords)</string></li> 
    <li>Query.ExtractTerms(ICollection<string>)</string></li> 
    <li>TopDocs.TotalHits, TopDocs.ScoreDocsv</li> 
  </ul>
