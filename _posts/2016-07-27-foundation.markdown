---
layout: post
title: The Apache Software Foundation Announces Apache® Mesos™ v1.0
date: '2016-07-27T00:00:00+00:00'
categories: foundation
---
<div><b><i>Mature Open Source cluster resource manager, container orchestrator, and distributed operating systems kernel in use at Netflix, Samsung, Twitter, and Yelp, among others.</i></b></div> 
  <div><b><br /></b></div> 
  <div><b>Forest Hill, MD —27 JULY 2016—</b> The Apache Software Foundation (ASF), the all-volunteer developers, stewards, and incubators of more than 350 Open Source projects and initiatives, announced today the availability of Apache® Mesos™ v1.0, the mature clustering resource management platform.</div> 
  <div><br /></div> 
  <div>Apache Mesos provides efficient resource isolation and sharing across distributed applications in Cloud environments as well as private datacenters. Mesos is a cluster resource manager, a container orchestrator, and a distributed operating systems kernel.</div> 
  <div><br /></div> 
  <div>&quot;At Berkeley in 2009, we were thinking about a new way to manage clusters and Big Data, and Mesos was born,&quot; said Benjamin Hindman, Vice President of Apache Mesos, one of the original creators of the project, and Chief Architect/Co-Founder of Mesosphere. &quot;Mesos v1.0 is a major milestone for the community.&quot;</div> 
  <div><br /></div> 
  <div>Mesos entered the Apache Incubator in 2010 and has had 36 releases since becoming a Top-Level Project (TLP) in 2013.</div> 
  <div><br /></div> 
  <div><b>Under The Hood</b></div> 
  <div>Apache Mesos 1.0 includes a number of new and important features that include:</div> 
  <div> 
    <ul> 
      <li><u>New HTTP API</u>: One of the main areas of improvement in the 1.0 release, this API simplifies writing Mesos frameworks by allowing developers to write frameworks in any language via HTTP. The HTTP API also makes it easy to run frameworks behind firewalls and inside containers.<br /><br /></li> 
      <li><u>Unified containerizer</u>: This allows frameworks to launch Docker/Appc containers using the Mesos containerizer without relying on docker daemon (engine) or rkt. The isolation of the containers is done using isolators.<br /><br /></li> 
      <li><u>CNI support</u>: The network/cni isolator has been introduced in the Mesos containerizer to implement the Container Network Interface (CNI) specification proposed by CoreOS. With CNI, the network/cni isolator is able to allocate a network namespace to Mesos containers and attach the container to different types of IP networks by invoking network drivers called CNI plugins.<br /><br /></li> 
      <li><u>GPU support</u>: Support for using Nvidia GPUs as a resource in the Mesos &quot;unified&quot; containerizer. This support includes running containers with and without filesystem isolation (i.e., running both imageless containers as well as containers using a Docker image)<br /><br /></li> 
      <li><u>Fine-grained authorization</u>: Many of Mesos' API endpoints have added authentication and authorization, so that operators can now control which users can view which tasks/frameworks in the web UI and API, in addition to fine-grained access control over other operator APIs such as reservations, volumes, weights, and quota.</li> 
      <li><u>Mesos on Windows</u>: Support for running Mesos on the Windows operating system is currently in beta. The Mesos community is aiming for full support by late 2016.</li> 
    </ul> 
  </div> 
  <div><br /></div> 
  <div>Over the years, Mesos has gained popularity with datacenter operators for being the first Open Source platform capable of running containers at scale in production environments, using both Docker containers and directly with Linux control groups (cgroups) and namespace technologies. Mesos' two-level scheduler distinguishes the platform as the only one that allows distributed applications such as Apache Spark, Apache Kafka, and Apache Cassandra to schedule their own workloads using their own schedulers within the resources originally allocated to the framework and isolated within a container.</div> 
  <div><br /></div> 
  <div>&quot;Initially the big breakthrough was this new way to run containers at scale, but the beauty of the design of Mesos and its two-level scheduler has proven to be its ability to run not only containers, but Big Data frameworks, storage services, and other applications all on the same cluster,&quot; added Hindman. &quot;Mesos has become a core technology that serves as a kernel for other systems to be built on top, so the maturity on the API has been a big focus, and it’s one of the main areas of improvement in the 1.0 release.&quot;&nbsp;</div> 
  <div><br /></div> 
  <div>These capabilities have distinguished Apache Mesos as the kernel of choice for many Open Source and commercial offerings. One of Mesos' earliest and most notable users was Twitter, who leveraged the Mesos architecture to kill the &quot;Fail Whale&quot; by handling its massive growth in site traffic. Prominent Mesos contributors and users include IBM, Mesosphere, Netflix, PayPal, Yelp, and many more.</div> 
  <div><br /></div> 
  <div>&quot;We use Mesos regularly at NASA JPL - we are leveraging Mesos to manage cluster resources in concert with Apache Spark identify Mesoscale Convective Complexes (MCC) or extreme weather events in satellite infrared data. Mesos has performed well in managing a high memory cluster for our team,&quot; said Chris A. Mattmann, member of the Apache Mesos Project Management Committee, and Chief Architect, Instrument and Science Data Systems Section at NASA JPL. &quot;We have also taken steps to integrate the Apache OODT data processing framework used in our missions with Apache Mesos.&quot;</div> 
  <div><br /></div> 
  <div>Learn more about Apache Mesos at MesosCon Europe 2016 conference in Amsterdam 31 August-1 September 2016, and at MesosCon Asia 2016 in Hangzhou, China 18-19 November 2016.</div> 
  <div><br /></div> 
  <div><b>Availability and Oversight</b></div> 
  <div>Apache Mesos software is released under the Apache License v2.0 and is overseen by a self-selected team of active contributors to the project. A Project Management Committee (PMC) guides the Project's day-to-day operations, including community development and product releases. For downloads, documentation, and ways to become involved with Apache Mesos, visit <a href="http://mesos.apache.org/">http://mesos.apache.org/</a> and <a href="https://twitter.com/ApacheMesos">https://twitter.com/ApacheMesos</a></div> 
  <div><br /></div> 
  <div><b>About The Apache Software Foundation (ASF)</b></div> 
  <div>Established in 1999, the all-volunteer Foundation oversees more than 350 leading Open Source projects, including Apache HTTP Server --the world's most popular Web server software. Through the ASF's meritocratic process known as &quot;The Apache Way,&quot; more than 550 individual Members and 5,300 Committers successfully collaborate to develop freely available enterprise-grade software, benefiting millions of users worldwide: thousands of software solutions are distributed under the Apache License; and the community actively participates in ASF mailing lists, mentoring initiatives, and ApacheCon, the Foundation's official user conference, trainings, and expo. The ASF is a US 501(c)(3) charitable organization, funded by individual donations and corporate sponsors including Alibaba Cloud Computing, ARM, Bloomberg, Budget Direct, Cerner, Cloudera, Comcast, Confluent, Facebook, Google, Hortonworks, HP, Huawei, IBM, InMotion Hosting, iSigma, LeaseWeb, Microsoft, OPDi, PhoenixNAP, Pivotal, Private Internet Access, Produban, Red Hat, Serenata Flowers, WANdisco, and Yahoo. For more information, visit <a href="http://www.apache.org/">http://www.apache.org/</a> and <a href="https://twitter.com/TheASF">https://twitter.com/TheASF</a></div> 
  <div><br /></div> 
  <div>© The Apache Software Foundation. &quot;Apache&quot;, &quot;Mesos&quot;, &quot;Apache Mesos&quot;, &quot;Cassandra&quot;, &quot;Apache Cassandra&quot;, &quot;Kafka&quot;, &quot;Apache Kafka&quot;, &quot;OODT&quot;, &quot;Apache OODT&quot;, &quot;Apache Spark&quot;, &quot;Apache Spark&quot;, and &quot;ApacheCon&quot; are registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. All other brands and trademarks are the property of their respective owners.</div> 
  <div><br /></div> 
  <div># # #</div>
