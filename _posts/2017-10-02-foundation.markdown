---
layout: post
title: 'Success at Apache: All My Roads Led to Apache'
date: '2017-10-02T00:00:00+00:00'
categories: foundation
---
<div> 
    <p><strong><em>by Pat Ferrel</em></strong></p> 
    <p>I became involved with Apache in 2011. After several years in startups where, as CTO, I felt too removed from building things. Looking for a change, I was keenly aware that the most interesting thing about the startups was our early use of Machine Learning techniques and I wanted to see if building ML solutions, for companies new to the field might not be more satisfying. I started by spending nearly a year in researching the type of applications we had needed in the startups: Natural Language Processing (NLP), text analysis, clustering, and classification. In those days Apache Mahout <a href="http://mahout.apache.org/">http://mahout.apache.org/</a> had several good solutions that were designed for Big Data and approachable by an individual. These ideas seem fairly commonplace now but were in early days only 6 years ago.</p> 
  </div> 
  <div>Given a great platform to experiment with, I built a web site to advertise expertise in ML but also to showcase many examples from my experiments, including a topic-oriented content site based on clustered and classified text that used NLP to add entities to text. I blogged about things I had learned and techniques that produce results.</div> 
  <div><br /></div> 
  <div>Then I got the first contact about a project and it was from a completely unexpected direction: recommenders. Fortunately Apache Mahout then had the state-of-the-art OSS suite of recommenders so I took the consulting job. The company had rolled their own recommender and was selling it as a service but it was old and they wanted to investigate replacing it.&nbsp;</div> 
  <div> 
    <p><strong>Welcome to Big Data</strong></p> 
  </div> 
  <div>The nature of recommenders means you deal with huge amounts of data because you have to track several million people’s actions over years. We had data from a large online retailer and were tasked with using this data to beat the in-house recommender. Specifically they wanted to see if they could improve performance (better results and faster compute times) and get something easier to maintain.&nbsp;</div> 
  <div><br /></div> 
  <div>The first job of a good consultant is to define the problem and outline a path to resolution that fits with the company’s competencies. To me this meant looking at the current system and the expertise of the people working on it. We had Data Scientists and Java Software Developers who knew what it was like to deal with Big Data. They had a highly performant method for gathering data and were quite good at running Apache Hadoop-based analytics. This was seldom the case back then but happily allowed me to look at less turnkey applications and assume the use of important Apache tools.</div> 
  <div><br /></div> 
  <div>We agreed on a plan and the basic building blocks including a method for comparing results. I did the research and proposed several candidates for the tests including the Apache Mahout recommenders. It was pretty easy to rank the recommender engines we had and do some exploration of parameter tuning and choices to get our best &quot;challenger&quot; results. The nice thing is that we beat the old threadbare in-house recommender by a significant amount (12%). The winner was the Apache Mahout Cooccurrence Recommender using the Log-Likelihood Ratio as the core cooccurrence metric. This even though we had tested against several Matrix Factorization recommenders, including Mahout's.&nbsp;</div> 
  <div> 
    <p><strong>We need something new&nbsp;</strong></p> 
  </div> 
  <div>Up till this time I was only a user of Apache projects (discounting a few minor code contributions) but what I found in all recommenders we studied is a fundamental problem that is still mostly unsolved today. We had data from a retailer that included user &quot;buys&quot;&nbsp;but also 100 times more user &quot;views&quot;. None of the recommenders could deal with this multimodal data. I consulted the authors and maintainers of the Mahout recommenders and several others we had targeted. We got some suggestions added them to our own ideas and set out to test them. For various reasons, that are beyond the scope of this post, none of the easy solutions helped and actually produced worse results so I had fulfilled the contract and left with a feeling of unfinished business.</div> 
  <div> 
    <p>One of the mentors of Apache Mahout, Ted Dunning, had suggested a new idea during this time. There was something about it that seemed very intriguing. He had proposed a way to use one type of user behavior to predict another. This was an aha moment for me because it codified intuition. I remember the first time he wrote in email on the Mahout user mailing list the equation that crystallized it all. I began to imagine the implications; all sorts of new data that could be useful, not just &quot;views&quot; but contextual data like location, and enrichment data like tag or category preferences. These all seem to obviously have a bearing on recommendations but now we had a beautiful simple equation to test the intuition.</p> 
    <p><strong>Becoming a Committer</strong></p> 
  </div> 
  <div>I set out to hack the Mahout Cooccurrence Recommender to become a Correlated Cross-Occurrence (CCO) recommender. But without some way of testing the algorithm and code we couldn’t be sure it was worth including in Mahout. The datasets publicly available at the time did not have the kind of data we needed (there had been no direct use for it until then) so I scraped the film review site rottentomatoes.com to collect &quot;fresh&quot; and &quot;rotten&quot; reviews of movies. This gave us two different behaviors with very different meanings. Naively you might think, weight one positive and the other negative and so did I but that produced worse results than ignoring the &quot;dislikes&quot;. However when I ran cross-validation tests comparing the Mahout Cooccurrence Recommender using likes only, to CCO using both user actions, we got some quite interesting results. The question was: do &quot;dislikes&quot; predict &quot;likes&quot; and when I got 20% lift in predictive precision we could conclude that they do. Not only was intuition right but the new algorithm could tease out the data to make use of it.</div> 
  <div> 
    <p>The hack was accepted into Mahout Examples and I was invited to become a committer. Then the world changed.</p> 
    <p><strong>Apache Spark and Mahout-Samsara</strong></p> 
  </div> 
  <div>When I became a committer Mahout was written on Apache Hadoop MapReduce in Java (as was my hack). But it had also become obvious to most Mahout committers that the future was with much more performant engines like Apache Spark. Committers Dmitriy Lyubimov and Sebastian Schelter had been working on a Spark version of Mahout. In an instant of project time virtually all committers saw this as the future of Mahout, if also a major pivot.&nbsp;</div> 
  <div><br /></div> 
  <div>In retrospect I'm not sure I've ever seen an Apache project change so much in so little time. Today Mahout is deprecating lots of old Hadoop MapReduce code as it falls from use and the new Mahout is truly new. The Mahout subtitle Samsara, references the cycle of life, death, and rebirth in the Hindu tradition. Mahout started as algorithms written specifically for MapReduce, now Mahout-Samsara is a linear algebra DSL in Scala used to roll-your-own algorithms but with most interesting algorithms in very simple DSL-based implementations. Mahout eventually took this transformation even further to include other compute engines like Apache Flink and is now running on GPUs. But I get ahead of things...</div> 
  <div> 
    <p>Those were exciting times and though I helped with the DSL I remained fixed on implementing CCO, which was first included in Mahout 0.10.0 in October 2014.</p> 
    <p><strong>PredictionIO</strong></p> 
  </div> 
  <div>Now we have the CCO algorithm implemented on modern compute engines but several other problems remained in order to actually deploy a recommender. This is because CCO creates a model that needs to be deployed on a special type of server that computes similarity in real time. In Machine Learning terms this is a K-Nearest Neighbors engine, known in concrete terms as Lucene, or it's scalable server derivatives like Solr and Elasticsearch. A turnkey recommender also requires a highly performant massively scalable DB, like HBase. Putting these together we could get a nearly turnkey recommendation server that made use of multimodal real time user behavior. But I didn't see a candidate for all these in Apache and so looked elsewhere. This required an integration project, not Mahout, which integrated with other services but provided none of its own.</div> 
  <div> 
    <p>I found a project that included everything I needed and was Apache licensed but was run by a small startup called PredictionIO. They had a Machine Learning Server that was a framework for Templates that could implement a wide range of Algorithms. The Server also included nice high-level integrations with Elasticsearch (Lucene server), Spark, and HBase. In May of 2015 I had the first running CCO Server build on Mahout and a whole list of other Apache projects. </p> 
    <p><strong>Back to Apache</strong></p> 
  </div> 
  <div>PredictionIO was at the right place to get swept up in a major move to embrace ML/AI by Salesforce Inc. who bought them as part of the Einstein initiative. Since PIO was Apache licensed OSS it was still available and so was the Template I was calling the Universal Recommender. But there was a question now about the future of PIO; what would Salesforce do with it? The old team, that I had worked closely with, wanted to see the project move forward in OSS and Salesforce seemed to agree, but large corporations often have a mixed record in promoting their own OSS projects. In this case Salesforce decided to remove the question by submitting PredictionIO to the Apache Incubator.</div> 
  <div><br /></div> 
  <div>The old team was joined by people like me from outside Salesforce to create a project that follows the Apache Way and is free of corporate dominance. I am a committer to PredictionIO, which has three releases under Apache Incubator vigilance and the Universal Recommender is now at v0.6.0, the most popular of PredictionIO Template Algorithms.</div> 
  <div> 
    <p>With the 3rd release of PIO from Apache we are now in the process of graduation to an Apache Top-Level Project, hatched by the Apache Incubator. I fully expect that we'll be celebrating soon.</p> 
    <p><strong>Postscript</strong></p> 
  </div> 
  <div>My journey began with a specific problem to solve. Each step to produce the solution has led back to Apache in one way or another, through mentors, collaboration, use of, and commitment to several projects. But I now have my mature scalable, performant, state-of-the-art nearly turnkey Universal Recommender. &nbsp;Now we can ingest and get improvements from many types of behavior, enrichment data, and context--using it in real time to serve recommendations subject to robust business rules. My small consulting company ActionML actionml.com now has a powerful tool to solve real problems and we make a living (at least partly) by helping people deploy and tune it for their data.</div> 
  <div> 
    <p>This is a story of someone single mindedly following a goal over several years. There are many ways to do this in the Software Development world, but not all OSS projects are open to bringing people in. The Apache Software Foundation most certainly is and openly recruits as diverse a group of committers and members as possible. If you want to make a difference and influence the course of an OSS project Apache is a good place to look. Start by getting involved with a project of interest, make contributions, get involved in discussions. If the match is good you'll be invited in as a committer and move on from there. I think of Apache as a do-ocracy, if you do something of value it goes a long way towards being invited in. &nbsp;</p> 
    <p><strong>References</strong></p> 
  </div> 
  <div> 
    <p>Slides describing the CCO Algorithm: <a href="https://www.slideshare.net/pferrel/unified-recommender-39986309">https://www.slideshare.net/pferrel/unified-recommender-39986309</a> </p> 
    <p>IBM DevWorks Post on &quot;Making one thing Predict Another&quot;: <a href="https://developer.ibm.com/dwblog/2017/mahout-spark-correlated-cross-occurences/">https://developer.ibm.com/dwblog/2017/mahout-spark-correlated-cross-occurences/</a></p> 
  </div> 
  <div> 
    <p>Apache Mahout CCO Implementation: <a href="http://mahout.apache.org/users/algorithms/intro-cooccurrence-spark.html">http://mahout.apache.org/users/algorithms/intro-cooccurrence-spark.html</a></p> 
    <p><a href="http://mahout.apache.org/users/algorithms/intro-cooccurrence-spark.html"></a>Apache PredictionIO: <a href="http://predictionio.incubator.apache.org/">http://predictionio.incubator.apache.org/</a></p> 
  </div> 
  <div> 
    <p>The Universal Recommender Template: <a href="http://predictionio.incubator.apache.org/gallery/template-gallery/">http://predictionio.incubator.apache.org/gallery/template-gallery/</a></p> 
    <p><a href="http://predictionio.incubator.apache.org/gallery/template-gallery/"></a>Professional Support for the Universal Recommender: <a href="http://actionml.com/universal-recommender">http://actionml.com/universal-recommender</a></p> 
  </div> 
  <div> 
    <p># # #</p> 
    <p>&quot;Success at Apache&quot; focuses on the processes behind why the ASF &quot;just works&quot;. 1) Project Independence <a href="https://s.apache.org/CE0V">https://s.apache.org/CE0V</a> 2) All Carrot and No Stick <a href="https://s.apache.org/ykoG">https://s.apache.org/ykoG</a> 3) Asynchronous Decision Making <a href="https://s.apache.org/PMvk">https://s.apache.org/PMvk</a> 4) Rule of the Makers <a href="https://s.apache.org/yFgQ">https://s.apache.org/yFgQ</a> 5) JFDI --the unconditional love of contributors <a href="https://s.apache.org/4pjM">https://s.apache.org/4pjM</a> 6) Meritocracy and Me <a href="https://s.apache.org/tQQh">https://s.apache.org/tQQh</a> 7) Learning to Build a Stronger Community <a href="https://s.apache.org/x9Be">https://s.apache.org/x9Be</a> 8) Meritocracy. <a href="https://s.apache.org/DiEo">https://s.apache.org/DiEo</a> 9) Lowering Barriers to Open Innovation <a href="https://s.apache.org/dAlg">https://s.apache.org/dAlg</a> </p> 
  </div>
