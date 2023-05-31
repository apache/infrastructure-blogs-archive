---
layout: post
title: The Apache Software Foundation Announces Apache® CloudMonkey® v6.0
date: '2019-03-20T00:00:00+00:00'
categories: foundation
---
<div><strong><em>Popular Open Source Command Line Interface tool that simplifies Apache CloudStack configuration and management now faster and easier to use.</em></strong></div> 
  <div><strong><em><br /></em></strong></div> 
  <div><strong><em>Wakefield, MA —20 March 2019—</em></strong> The Apache Software Foundation (ASF), the all-volunteer developers, stewards, and incubators of more than 350 Open Source projects and initiatives, announced today Apache® CloudStack® CloudMonkey v6.0, the latest version of the turnkey enterprise Cloud orchestration platform's command line interface tool.&nbsp;</div> 
  <div><br /></div> 
  <div>Apache CloudStack is the proven, highly scalable, and easy-to-deploy IaaS platform used for rapidly creating private, public, and hybrid Cloud environments. Thousands of large-scale public Cloud providers and enterprise organizations use Apache CloudStack to enable billions of dollars worth of business transactions annually across their clouds.</div> 
  <div> 
    <p>Apache CloudMonkey v6.0.0 is the latest major release since the previous major 5.x release in September 2013.&nbsp; CloudMonkey v6.0.0 is a rewrite of the original tool in Go programming language, and can be used both as an interactive shell and as a command line tool that simplifies CloudStack configuration and management.</p> 
  </div> 
  <div>Some of the new features and major changes include:</div> 
  <div> 
    <ul> 
      <li>Rewrite in Go, ships as single binary for Linux, Mac, and Windows</li> 
      <li>Drop-in replacement for legacy Python-based cloudmonkey</li> 
      <li>About 5-20x faster than legacy Python-based cloudmonkey</li> 
      <li>Interactive UX for parameter and arg completion and selection</li> 
      <li>JSON is the default output format</li> 
      <li>New column based output</li> 
      <li>Enable debug mode using set debug true option, file-based logging removed</li> 
      <li>Per server profile based API cache</li> 
      <li>New syntax arg=@/path/to/file to pass the content of file as API argument value similar to curl</li> 
      <li>Improve help docs using -h argument</li> 
      <li>Removed: XML output, coloured output, several set options</li> 
    </ul> 
  </div> 
  <div><br />&quot;This release is the work of over one year of effort and driven by the people operating CloudStack clouds,&quot; said Rohit Yadav, Apache CloudStack CloudMonkey v6.0 author, and release manager. &quot;I would like to thank the contributors across all of these organizations for supporting this release, which reflects both the user-driven nature of our community and the Apache CloudStack project's commitment to continue to be the most stable, easily deployable, scalable Open Source platform for IaaS. Along with ease of installation, usage and availability of cross-platform dependency-free builds including Windows builds, v6.0 brings many changes and optimizations such as more interactive shell for parameter completion, faster API requests processing, server profile specific API caching, improved API help docs and a new syntax to pass content of files as API parameter argument.&quot; More on the background and story behind the CloudMonkey 6.0 effort can be found at <a href="The%20Apache%20Software%20Foundation%20Announces%20Apache%AE%20CloudMonkey%AE%20v6.0">https://blogs.apache.org/cloudstack/entry/what-s-coming-in-cloudmonkey</a></div> 
  <div><br /></div> 
  <div>&quot;Apache CloudStack is a significant part of our Cloud portfolio right now – we run large deployments all over the world, often supporting critical customer applications,&quot; said Robert van der Meulen, Product Strategy Lead at Leaseweb Global B.V. &quot;CloudMonkey is an invaluable tool for interacting with CloudStack-based clouds, and it's the go-to tool that we recommend to our customers when they want to use command-line interaction with our CloudStack platforms.&quot;</div> 
  <div><br /></div> 
  <div>&quot;CloudMonkey is an effective tool for the operators of CloudStack environments and&nbsp; it becomes essential in large-scale CloudStack deployments,&quot; said Giles Sirett, CEO of ShapeBlue. &quot;It's great to see this new version of CloudMonkey: having a CLI that can run on Windows desktops as well as Linux and Mac is important as we see more enterprise adoption of Apache CloudStack.&quot;</div> 
  <div><br /></div> 
  <div>&quot;CloudMonkey is now written in Golang, and with version v6.0 loading, speed has been drastically improved (accessing the CLI in under 0.5s),&quot; said Pierre-Luc Dion, Cloud Architect at Cloud.ca. &quot;This simplifies installation, deployments, updates, and operational efficiency.&quot;</div> 
  <div><br /></div> 
  <div>&quot;After many years of managing production Apache CloudStack deployments, I consider CloudMonkey a core tool in anyone's CloudStack toolkit, and now also being available for Windows makes me really happy,&quot; said Andrija Panic, Apache CloudStack Committer. &quot;I can certainly see major speed improvements, but also having backward compatibility is what is so great with this new release.&quot;</div> 
  <div><br /></div> 
  <div>Catch Apache CloudStack in action at ApacheCon 9-12 September 2019 in Las Vegas, Nevada, and at numerous Meetups worldwide, held throughout the year.</div> 
  <div><br /></div> 
  <div><strong>Downloads and Documentation</strong></div> 
  <div>The official source code for CloudMonkey v6.0.0 can be downloaded from http://cloudstack.apache.org/downloads.html. The community-maintained builds are available at the project's Github release page at https://github.com/apache/cloudstack-cloudmonkey/releases . CloudMonkey's usage is documented at <a href="https://github.com/apache/cloudstack-cloudmonkey/wiki">https://github.com/apache/cloudstack-cloudmonkey/wiki</a> </div> 
  <div><br /></div> 
  <div><strong>Availability and Oversight</strong></div> 
  <div>Apache CloudStack software is released under the Apache License v2.0 and is overseen by a self-selected team of active contributors to the project. A Project Management Committee (PMC) guides the Project's day-to-day operations, including community development and product releases. For downloads, documentation, and ways to become involved with Apache CloudStack, visit <a href="http://cloudstack.apache.org/">http://cloudstack.apache.org/</a> and <a href="https://twitter.com/CloudStack">https://twitter.com/CloudStack</a> </div> 
  <div><br /></div> 
  <div><strong>About The Apache Software Foundation (ASF)</strong></div> 
  <div>Established in 1999, the all-volunteer Foundation oversees more than 350 leading Open Source projects, including Apache HTTP Server —the world's most popular Web server software. Through the ASF's meritocratic process known as &quot;The Apache Way,&quot; more than 730 individual Members and 7,000 Committers across six continents successfully collaborate to develop freely available enterprise-grade software, benefiting millions of users worldwide: thousands of software solutions are distributed under the Apache License; and the community actively participates in ASF mailing lists, mentoring initiatives, and ApacheCon, the Foundation's official user conference, trainings, and expo. The ASF is a US 501(c)(3) charitable organization, funded by individual donations and corporate sponsors including Aetna, Alibaba Cloud Computing, Anonymous, ARM, Baidu, Bloomberg, Budget Direct, Capital One, Cerner, Cloudera, Comcast, Facebook, Google, Handshake, Hortonworks, Huawei, IBM, Indeed, Inspur, Leaseweb, Microsoft, ODPi, Pineapple Fund, Pivotal, Private Internet Access, Red Hat, Target, Tencent, Union Investment, Workday, and Verizon Media. For more information, visit <a href="http://apache.org/">http://apache.org/</a> and <a href="https://twitter.com/TheASF">https://twitter.com/TheASF</a> </div> 
  <div><br /></div> 
  <div><br /></div> 
  <div>© The Apache Software Foundation. &quot;Apache&quot;, &quot;CloudStack&quot;, &quot;Apache CloudStack&quot;, and &quot;ApacheCon&quot; are registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. All other brands and trademarks are the property of their respective owners.</div> 
  <div><br /></div> 
  <div># # #</div>
