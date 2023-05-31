---
layout: post
title: 'Success at Apache: What You Need to Know'
date: '2019-03-26T00:00:00+00:00'
categories: foundation
---
<div> 
    <p>EDITOR'S NOTE: <em>I came across the author's original post, &quot;An Introduction to Apache Software — What you need to know&quot;, dated 3 February 2017, and was interested in finding away to share with the greater Apache community. The author's enthusiasm was palpable, and earnestly intended to help educate others. With the ASF celebrating its 20th Anniversary this year, it's easy for many of us to simply rely on tribal knowledge, not realizing that navigation to definitive guides aren't intuitive to newcomers. Those of us who have been here for a while &quot;just know&quot;, partially because we were creating it as we went along. Below is an updated version of the original post, amended through the guidance of three long-standing ASF Members. And that's the point of it all at the end of the day: at Apache, we help each other as it contributes to our collective success, and this writeup will help others find their Success at Apache.&nbsp;</em></p> 
    <p><strong><em>by Maximilian Michels</em></strong> </p> <img src="https://blogs.apache.org/foundation/mediaresource/9674db93-b9cc-4f78-b81e-468d5c975fb2" align="left" /> 
    <p>Before you started reading this post, you have already been using Apache software. The Apache web server (Apache HTTP Server) serves about every second web page on the WWW, including this website. One could say, Apache software runs the WWW. But it doesnt stop there. Apache is more than a web server. Apache software also runs on mobile devices. Apache software is part of enterprise and banking software. Apache software is literally everywhere in today's software world.</p> 
    <p>Apache has become a powerful brand and a philosophy of software development which remains unmatched in the world of open-source. Although the Apache® trademark is a known term even among the less tech-savvy people, many people struggle to define what Apache software really is about, and what role it plays for today's software development and businesses.</p> 
    <p>In the last years I've learned a lot about Apache through my work on <a href="http://flink.apache.org/">Apache Flink</a> and <a href="http://beam.apache.org/">Apache Beam</a> with dataArtisans, as a freelancer/consultant, and as a volunteer. In this post I want to give an overview of the Apache Software Foundation and its history. Moreover, I want to show how the &quot;Apache way&quot; of software development has shaped the open-source software development as it is today.</p> 
    <p> </p> 
    <p> </p> 
    <div> 
      <p> </p> 
      <p> </p> 
      <h2><strong>The History of the Foundation</strong></h2> 
      <p><img src="https://blogs.apache.org/foundation/mediaresource/8aa1c286-6d18-4b1a-b875-b40a949b2085" align="left" />The Apache Software Foundation (ASF) was founded in 1999 by a group of open-source enthusiasts who saw the need to create a legal entity to institutionalize their work. Among the first projects was the famous web server called Apache HTTP, which is also simply referred to as &quot;Apache web server&quot;. At that time, the Apache web server was already quite mature. In fact, not only did the Apache web server give the foundation its name but it became the role model for the &quot;Apache way&quot; of open and collaborative software development. To see how that took place, we have to go back a bit further in time.</p> 
      <p> </p> 
      <h3><strong>A Web Server goes a long way</strong></h3> 
      <p>As early as 1994, Rob McCool at the National Center for Supercomputing Applications (NCSA) in Illinois created a simple web server which served pages using one of the early versions of today's HTTP protocol. Web servers were not ubiquitous like they are today. In these days, the Web was still in its early days and there was only one web browser developed at CERN where the WWW was invented only shortly before. Rob's web server was adopted quite fruitfully throughout the web due to its extensible nature. When its source code spread, web page administrators around the world developed extensions for the web server and helped to fix errors. When Rob left the NCSA in late 1994, he also left a void because there was nobody left to maintain the web server along with its extensions. Quickly it became apparent that the group of existing users and developers needed to join forces to be able to maintain NCSA HTTP.</p> 
      <p>At the beginning of 1995, the Apache Group was formed to coordinate the development of the NCSA HTTP web server. This led to the first release of the Apache web server in April 1995. During the same time, development at NCSA started picking off again and the two teams were in vivid exchange about future ideas to improve the web server. However, the Apache Group was able to develop its version of the web server much faster because of their structure which encouraged worldwide collaboration. At the end of the year, the Apache server had its architecture redone to be modular and it executed much faster.</p> 
      <p>One year later, at the beginning of 1996, the Apache web server already succeeded the popularity of the NCSA HTTP which had been the most popular web server on the Internet until then. Apache 1.0 finally was released on Dec 1, 1995. The web server continued to thrive and is still the most widely used web browser as of this writing.</p> 
      <h3><strong>The Rise of the Foundation</strong></h3> 
      <p><img src="https://blogs.apache.org/foundation/mediaresource/80177034-f430-4fca-af6d-c921dbeaf39d" align="left" /></p> 
      <p>The team effort that led to the development and adoption of the Apache web server was a huge success. The Apache project kept receiving feedback and code changes (also called patches) from people all over the world. Could this be the development model for future software? More and more projects started to organize their groups similarly to the Apache group. As the number of project grew, financial interests arose and potential legal issues threatened the existence of the Apache group. Out of this need, the Apache Software Foundation (ASF) was incorporated as a US 501(c)(3) non-profit organization in June 1999. In the US, the 501(c)(3) is a legal entity specifically designed for non-profit charitable organizations. This is in contrast to other for-profit open-source software organizations or even US 501(c)(6) non-profit organizations which do not require to be charitable.</p> 
      <p>After the ASF was incorporated, new projects could easily leverage the foundation's services. Over the next year, <a href="https://projects.apache.org/committees.html?date">every few months a new project entered the ASF</a>. The first projects after Apache HTTP Server were Apache mod_perl (March 2000), Apache tcl (July 2000), and Apache Portable Runtime (December 2000). After a short break in 2001 which was used to come up with a programmatic approach to onboard new projects via an incubator, the ASF has seen very consistent growth of up to 12 projects (2012) each year.</p> 
      <p>The ASF became a framework for open-source software development which, in its entirety, remains unmatched by other forms of open-source software development. The secret of ASF's success is its unique approach to scaling its operations, in which the foundation does not try to exercise control over its projects. Instead, it focuses on providing volunteers with the infrastructure and a minimal set of rules to manage their projects. The projects itself remain almost autonomous.</p> 
      <h2><strong>Apache Governance - How does the foundation work?</strong></h2> 
      <p>There are about <a href="http://projects.apache.org/">200 independent projects</a> running under the Apache umbrella. The question may arise, how does the foundation govern its projects? First of all, the ASF is an organization that is run almost entirely by volunteers. In the early days, many of the volunteers were developers which did not like to spend much time with administrative things (who does?), so the organization is structured in a way that requires little central control but favors autonomy of the projects which run under its umbrella.</p> 
      <h3><strong><em>Per-Project Entities</em></strong></h3> 
      <p>For every project (e.g. Apache HTTP, Apache Hadoop, Apache Commons, Apache Flink, Apache Beam, etc.), there are a Project Management Committee (PMC), Committers, Contributors, and Users.</p> <img src="https://blogs.apache.org/foundation/mediaresource/ad5d2fd4-f285-417c-8947-594f3e0e18da" align="left" /> 
      <h4><em><u>Project Management Committee (PMC)</u></em></h4> 
      <p>The PMC manages a project's community and decides over its development direction. Its most rudimentary and traditional role is to approve releases for a project. In that sense it has a similar function as the original Apache Group which led the development of Apache HTTP Server. When a new project graduates from the Incubator (covered later), the foundation's central instance, the Board, approves the initial PMC which is selected from the PPMC (Podling PMC) formed during incubation. Each PMC elects one PMC member as the PMC Chair which represents the project and writes quarterly reports to the ASF Board. The Chair needs to be approved by the Board.</p> 
      <p>Through a project's lifetime new PMC members can be elected by the existing PMC. Note that each new PMC member needs to be approved by the Board but this approval is merely formal and there are few instances that a new PMC member is not approved. PMC members do not need the formal permission of the foundation to elect new Committers. PMC members themselves are also Committers. Let's learn about Committers next.</p> 
      <h4><u><em>Committers</em></u></h4> 
      <p>Committers can modify the code base of the project but they can't make decisions regarding the governance of the project. They are trusted by the PMC to work in the interest of the project. When they contribute changes, they commit (thus, the name) these changes to the project. Committers don't only change code but they can also update documentation, write blog posts on the project's website, or give talks at conferences. Committers are selected from the users of the project; more about this process in the Meritocracy section.</p> 
      <div> 
        <h4><u><em>Users and Contributors</em></u></h4> 
        <div> 
          <p>Users are as important as the developers because they try out the project’s software, report bugs, and request new features. The term is a slightly confusing because, in the Apache world, most users tend to be developers themselves. They are users in the sense that they are using an Apache project for their own work; usually they are not actively developing the Apache software they are using. However, they may also provide patches to the Committers. Users who contribute to a project are called Contributors. Contributors may eventually become Committers.</p> 
          <p>In the image, the per-project entities are represented as circles. They exist for every project. Note that the user group circle is not depicted in full size because big projects tend to have much more Users than Committers and PMC members.</p> 
        </div> 
      </div> 
      <div> 
        <div> 
          <h3><strong>Foundation-Wide Entities</strong></h3> 
          <p>The ASF does not work without some central services. Here are the most important entities:</p> 
        </div> 
        <h4><em><u>Apache Members</u></em></h4> 
        <p>Apache members represent the heart of the foundation. They have been referred to as the &quot;shareholders of the ASF&quot; because they are deeply invested in the ASF (not in the financial sense). A prerequisite to becoming a member is to be active in at least one project. To become a member, you have to show interest in the foundation and try to promote its values. The ASF holds membership meetings which are usually held annually. At membership meetings new members can be proposed and subsequently elected. Elected members receive an invitation which they can choose to accept within 30 days. Becoming a member it not merely a recognition for supporting the ASF, but it also grants the right to elect the Board.</p> 
      </div> 
      <div> 
        <h4><u><em>The Board of Directors (Board)</em></u></h4> 
        <p>The Board takes care of the overall government of the foundation. In particular, it is concerned with legal and financial matters like brand and licensing issues, fundraising, and financial planning. The board is elected by the Apache members annually and is also composed of Apache members. The current board can be viewed <a href="https://www.apache.org/foundation/board/">here</a>. Note that there is only one central Board for the entire foundation but Board members can be PMC members in different projects.</p> 
      </div> 
      <div> 
        <p><img src="https://blogs.apache.org/foundation/mediaresource/81abdbce-9b2d-4a1d-90e0-d5f4f55b7ca3" /></p> 
        <h4><u><em>Officers of the corporation</em></u></h4> 
        <p>The <a href="http://www.apache.org/foundation/#who-runs-the-asf">Officers of the corporation</a> are the executive part of the administration. They execute the decisions of the board and take care of everyday business. Most of the officers are implicitly officers by being the PMC chair of a project. Additionally, there are dedicated officers for central work of the foundation, e.g. fundraising, marketing, accounting, data privacy, etc.</p> 
        <h4><strong>Infrastructure (INFRA)</strong></h4> 
        <p>The support and administration team (INFRA) is the team that runs the Apache infrastructure and provides tools and support for developers. INFRA is the only team at Apache which consists of contractors which are paid for their work. Their work includes running the <a href="http://apache.org/">apache.org</a> web site and the mailing lists which are Apache’s main way of communication. Over time, various other tools and services were created to assist the projects. The main tools available which are used by almost all projects are:</p> 
        <p> </p> 
        <ul> 
          <li>Web space for the project's websites.</li> 
          <li>Mailing lists, for discussing the roadmap of the project, exchanging ideas, or reporting bugs (unwanted software behavior). Typically the mailing lists are divided into a developer and a user mailing list.</li> 
          <li>Bug trackers, which help developers to keep track of new features or bugs.</li> 
          <li>Version control, which helps developers to keep track of the code changes.</li> 
          <li>Build servers, which help to integrate/test new code or changes to existing code.</li> 
        </ul> 
        <p> </p> 
        <h4><strong>The Incubator</strong></h4> 
        <p>Founded in 2002, the Incubator is a project at the ASF dedicated to forming (bootstrapping) new Apache projects. The process is the following: People (volunteers, enthusiasts, or company employees) make a proposal to the Incubator. The proposal contains the project name, the list of initial PPMC (Podling PMC) members, and the motivation and goals for the new project. Once the IPMC (Incubator PMC) has discussed the proposal, it holds a vote to decide if the project enters the incubation phase. In the incubation phase, projects carry &quot;incubating&quot; in their names, e.g. &quot;Apache Flink (incubating)&quot;; this is dropped only once they graduate. To graduate, a project has to show that it is mature enough. The Community Development project at the ASF has created a catalogue of criteria called the <a href="https://community.apache.org/apache-way/apache-project-maturity-model.html">Maturity Model</a>. It requires having an active community, quality of code, and being legally compliant. Formally, the project needs to prove it fulfils the criteria to the Incubator IPMC which is comprised of Apache members. All existing work donated in the course of entering the incubator and all future work inside the project has to be licensed to the ASF under the Apache License. This ensures that development remains in the open-source according to the Apache philosophy. <a href="https://incubator.apache.org/">More about incubation on the official website</a>.</p> 
        <p> </p> 
        <h3><strong>Meritocracy - How are decisions made?</strong></h3> 
        <p> </p> 
        <p>The Apache Software Foundation uses the term &quot;meritocracy&quot; to describe how it governs itself. Going back to the ancient Greeks, meritocracy was a political system to put those into power which proved that they were motivated, put effort into their work, and were able to help a project. The core of this philosophy can be found throughout history from ancient China to medieval Europe and is still present in many of today’s cultures in the sense that effort, increased responsibility, and service to a part of society ought to pay off in terms of power of decision, social status, or money.</p> 
        <p>Meritocracy in the Apache Software Foundation denotes that people who either work in the interest of the foundation or a project get promoted. Users who submit patches may be offered Committer status. Comitters who are drive a project, may gain PMC status. PMC members active across projects and taking part in the foundation's work may earn the Member status.</p> 
        <p>Decision-making within the foundation and projects are typically performed using Consensus. Consensus can be &quot;lazy&quot; which implies that even a few people can drive a discussion and make decisions for the entire community as long as nobody objects. The discussions have to be held in public on the mailing list. For instance, if a Committer decides to introduce a new feature X, she may do so by proposing the feature on the mailing list. If nobody objects, she can go ahead and develop the feature. If lazy consensus does not work because an argument cannot be settled, a majority based vote can be started.</p> 
        <p>Meritocracy and &quot;lazy&quot; Consensus are the core principles for governance within the Apache Software Foundation. Meritocracy ensures that new people can join those already in power. &quot;Lazy&quot; Consensus creates the opportunity to split up decision-making among the group such that it doesn't always require the action of all members of the community.</p> 
        <div> 
          <div> 
            <h3>The Apache License - A license for the world of open-source</h3><br /> 
          </div> 
          <div>With the incorporation of the foundation in 1999, a license had to be created to prevent conflicts with the intellectual property contributed by others to the ASF. Originally, the license was meant to be used exclusively by the ASF but it quickly became one of the most widely used software licenses for all kinds of open-source software development.</div> 
          <div> 
            <p>The Apache license is very permissive in the sense that source code modifications are not required to be open-sourced (made publicly available) even when the source code is distributed or sold to other entities. This is in contrast to “Copyleft” licenses like the GNU Public License (GPL) which, upon redistribution, requires public attribution and publication of changes made to the original source code. The Apache license was first derived from the BSD license which is similarly permissive. The reason for this was that the Apache HTTP Server was originally licensed under the BSD license.</p> 
            <p>The current version of the Apache License is 2.0, released in January 2004. The changes made since the initial release are only minor but they set the prerequisite for its prevalence. At first, the license was only available to Apache projects. Due to the success of the Apache model, people also wanted to use the license outside the foundation. This was made possible in version 2.0. Also, the new version made it possible to combine GPL code with Apache licensed code. In this case, the resulting product would have to be licensed under the GPL to be compatible with the GPL license. Another change for version 2.0 was to make inclusion of the license in non Apache licensed projects easier and require explicit patent grants for patent-relevant parts.</p> 
            <h2>Apache Today</h2> 
          </div> 
        </div> 
        <div> 
          <p><img src="https://blogs.apache.org/foundation/mediaresource/16c3001d-735b-40ca-9a07-ee355eb06b80" /></p> 
          <p>The ASF today is not the small group that it used to be back in 1999. At the time of this writing, the Apache Software Foundation hosts 51 podlings in the Incubator and 199 top-level committees (PMCs). This amounts to almost 300 projects (<a href="https://projects.apache.org/">latest statistics</a>). Note that, a PMC may decide to host multiple projects if necessary. For instance, the Apache Commons PMC has split up the different parts of the Apache Commons library into separate projects (e.g. CLI, Email, Daemon, etc.). 50 of the 300 projects have been retired and are now part of <a href="https://attic.apache.org/">Apache Attic</a>, the project which hosts all retired projects. The above graph is taken from <a href="https://projects.apache.org">https://projects.apache.org</a>.</p> 
          <p><strong>Apache Conferences</strong></p> 
          <p>The Apache Software Foundation regularly organizes conferences around the world called <a href="http://www.apachecon.com/">ApacheCon</a>. These conferences are dedicated to the Apache community or certain topics like Big Data or IoT. It is a place to meet community members and learn about the latest ideas and trends within the global Apache community. Apart from the official conferences, there are conferences on Apache software organized by companies or external organization, e.g. <a href="http://www.stratahadoopworld.com/">Strata</a>, <a href="http://flink-forward.org/">FlinkForward</a>, <a href="https://kafka-summit.org/">Kafka Summit</a>, <a href="https://spark-summit.org/">Spark Summit</a>.</p> 
          <p>Here's a list of some projects that I came across in the past. I grouped them into categories for a better overview. I realize you might not know a lot of the projects but maybe this list can be the starting point to discover more about these Apache projects :)</p> 
          <p> </p> 
        </div> 
        <div> 
          <h3>Big Data</h3> 
          <div> 
            <ul> 
              <li>Hadoop</li> 
              <li>Flink</li> 
              <li>Spark</li> 
              <li>Beam</li> 
              <li>Samza</li> 
              <li>Storm</li> 
              <li>NiFi</li> 
              <li>Kafka</li> 
              <li>Flume</li> 
              <li>Tez</li> 
              <li>Zeppelin</li> 
            </ul> 
          </div> 
        </div> 
        <div> 
          <h3>Cloud</h3> 
          <div> 
            <ul> 
              <li>Mesos</li> 
              <li>CloudStack</li> 
              <li>Libcloud</li> 
            </ul> 
          </div> 
        </div> 
        <div> 
          <h3>Machine Learning</h3> 
          <div> 
            <ul> 
              <li>Mahout</li> 
              <li>SAMOA</li> 
            </ul> 
          </div> 
          <div> 
            <h3>Office</h3> 
          </div> 
          <div> 
            <ul> 
              <li>OpenOffice</li> 
            </ul> 
          </div> 
        </div> 
        <div> 
          <h3>Database</h3> 
          <div> 
            <ul> 
              <li>CouchDB</li> 
              <li>HBase</li> 
              <li>Zookeeper</li> 
              <li>Derby</li> 
              <li>Cassandra</li> 
            </ul> 
          </div> 
          <h3>Query Tools / APIs</h3> 
          <div> 
            <ul> 
              <li>Hive</li> 
              <li>Pig</li> 
              <li>Drill</li> 
              <li>Crunch</li> 
              <li>Ignite</li> 
              <li>Solr</li> 
              <li>Lucene</li> 
            </ul> 
          </div> 
          <h3>Programming Languages</h3> 
          <div> 
            <p> </p> 
            <ul> 
              <li>Groovy</li> 
            </ul> 
            <p> </p> 
            <h3>Distributions</h3> 
          </div> 
          <div> 
            <ul> 
              <li>Bigtop</li> 
              <li>Ambari</li> 
            </ul> 
          </div> 
        </div> 
        <div> 
          <h3>Libraries</h3> 
          <div> 
            <ul> 
              <li>Commons</li> 
              <li>Avro</li> 
              <li>Thrift</li> 
              <li>ActiveMQ</li> 
              <li>Parquet</li> 
            </ul> 
          </div> 
          <div> 
            <h3>Developer Tools</h3> 
          </div> 
          <div> 
            <ul> 
              <li>Ant</li> 
              <li>Maven</li> 
              <li>Ivy</li> 
              <li>Subversion</li> 
            </ul> 
          </div> 
          <div> 
            <h3>Web Servers</h3> 
          </div> 
          <div> 
            <ul> 
              <li>HTTP (the one!)</li> 
              <li>Tomcat</li> 
            </ul> 
          </div> 
          <p><span style="font-family: arial, verdana, &quot;Bitstream Vera Sans&quot;, helvetica, sans-serif; font-size: 14px; font-weight: bold; letter-spacing: -0.018em;">Web Frameworks</span></p> 
          <div> 
            <ul> 
              <li>Cocoon</li> 
              <li>Struts</li> 
              <li>Sling</li> 
            </ul> 
          </div> 
        </div> 
        <div> 
          <div><br /> 
            <h2>Apache - A Successful Open-Source Development Model</h2> 
          </div> 
          <div> 
            <p>My first attempt to learn more about Apache goes back several years. I was using the Apache License while working on <a href="http://scalaris.zib.de/">Scalaris</a> at <a href="http://zib.de/">Zuse Institute Berlin</a>. I realized that the license was somehow connected to the Apache Software Foundation but I didn't really understand the depth of this relationship until I started working on Apache Flink with dataArtisans. Besides the official homepage of the foundation, relatively little information was available on the Internet about the foundation and its projects. In hindsight, the best source of information was to read the email archives, get to know other people at the ASF, and become a volunteer myself :)</p> 
            <p>When I originally wrote this post I couldn’t find an introductory guide to the ASF. So I decided to do a bit of research myself and tried to write down what I had learned working on Apache projects. I hope that I could provide an overview of the ASF and show you how significant the foundation has been for the open-source software development.</p> 
          </div> 
          <h2>Thank you</h2> 
          <div> 
            <p>Thank you for reading this article. Feel free to <a href="https://maximilianmichels.com/about">write me an email</a> if I got something wrong or you would like to comment on anything.</p> 
            <p>Thank you Roman Shaposhnik, Shane Curcuru, Dave Fisher, and Sally Khudairi for your comments which were very helpful to revise this post for the 20th anniversary of the ASF.</p> 
          </div> 
          <h2>Sources</h2> 
          <div> 
            <ul> 
              <li><a href="https://w3techs.com/technologies/history_overview/web_server">Web Server usage</a></li> 
              <li><a href="http://apache.org/foundation/">Apache Software Foundation</a></li> 
              <li><a href="https://www.apache.org/history/">Apache History</a></li> 
              <li><a href="http://httpd.apache.org/ABOUT_APACHE.html">About Apache</a></li> 
              <li><a href="https://en.wikipedia.org/wiki/Robert_McCool">Robert McCool</a></li> 
              <li><a href="https://en.wikipedia.org/wiki/CERN_httpd">CERN httpd</a></li> 
              <li><a href="https://en.wikipedia.org/wiki/Apache_HTTP_Server">Apache HTTP</a></li> 
              <li><a href="https://www.apache.org/foundation/press/pr_1999_06_30.html">Initial press release</a></li> 
              <li><a href="https://en.wikipedia.org/wiki/Apache_License">Apache License</a></li> 
              <li><a href="https://www.apache.org/licenses/">Apache License Version 2.0</a></li> 
              <li><a href="https://www.apache.org/foundation/how-it-works.html">How it works</a></li> 
              <li><a href="https://www.apache.org/dev/">Apache Dev</a></li> 
              <li><a href="https://incubator.apache.org/">Apache Incubator</a></li> 
              <li><a href="https://projects.apache.org/">Apache projects</a></li> 
            </ul> 
          </div> 
          <p> </p> 
          <div> 
            <p>&nbsp;# # #</p> 
            <p><em>&quot;Success at Apache&quot; is a monthly blog series that focuses on the people and processes behind why the ASF &quot;just works&quot;. <a href="https://blogs.apache.org/foundation/category/SuccessAtApache">https://blogs.apache.org/foundation/category/SuccessAtApache</a></em></p> 
            <p> </p> 
          </div> 
        </div> 
      </div> 
    </div> 
    <p> </p> 
  </div>
