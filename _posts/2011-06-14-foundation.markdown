---
layout: post
title: Apache Traffic Server v3.0.0 Features-At-A-Glance
date: '2011-06-14T00:00:00+00:00'
categories: foundation
---
<div>In the works over the past year (v2.0 was released May 2010), Apache Traffic Server v3.0.0 is the result of contributions from more than 30 developers and contributors. Technical highlights include:</div> 
  <p>I. Contributions</p> 
  <div>- Over 1,000 &quot;commits&quot; (code, patches, or documentation written directly to the code repository);</div> 
  <div>- 380+ bug tickets resolved;</div> 
  <div>- 10 releases have been made during the development phase (v2.1.0 - 2.1.9)</div> 
  <div><br /></div> 
  <div>II. New features/improvements</div> 
  <div><br />- Full 64-bit support;</div> 
  <div>- Client side IPv6 support;</div> 
  <div>- WCCP (Web Cache Communication Protocol);</div> 
  <div>- Clustering is functional and supported (many benefits include an efficient distributed cache);</div> 
  <div>- Major plug-in API improvements, making the APIs more feature rich and easier to use;</div> 
  <div>- Support for many platforms, including OSX, Solaris and FreeBSD (Linux, of course, was always supported);</div> 
  <div>- New improved RAM cache algorithms, for better performance and memory utilization</div> 
  <div>- Many configurations are now configurable per transaction (or per mapping rule)</div> 
  <div>- Many improvements in statistics and management APIs;</div> 
  <div>- Multiple accept threads, and a dedicated DNS thread:</div> 
  <div>- Build environment is now much more flexible, and package owner friendly;</div> 
  <div>- Many, many bug fixes for improved stability and functionality.</div> 
  <div><br /></div> 
  <div>III. Performance improvements</div> 
  <div><br />- Overall throughput is 2-3x improved over v2.0 (depending on the traffic patterns)</div> 
  <div>- Response latency is up to 5x better than v2.0</div> 
  <div>- Benchmark: Serving small objects out of RAM cache, a high end server has showed over 220,000 requests / second</div> 
  <div>- Benchmark: Serving small objects that are not cacheable, the same high end server could proxy 100,000 requests / second</div> 
  <div>(NOTE: all benchmarks are on local network, with keep-alive)</div> 
  <div><br /><br /></div> 
  <div>Get Involved! Apache Traffic Server downloads, documentation, mailing lists, and related resources are available at <a href="http://trafficserver.apache.org/">http://trafficserver.apache.org/</a>.</div> 
  <p># # #</p>
