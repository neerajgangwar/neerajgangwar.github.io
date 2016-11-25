---
layout: post
title: A brief Introduction to Compressive Sensing
date: 2013-10-14 10:05:00
author: Neeraj Gangwar
comments: true
tags:
- sparse signals
- sampling
- compressed sensing
- compressive sensing
- cs
modified_time: '2014-03-24T07:24:06.211-07:00'
blogger_id: tag:blogger.com,1999:blog-8995384349089948093.post-7943451949913252987
blogger_orig_url: http://neerajgangwar.blogspot.com/2013/10/a-brief-introduction.html
---

<h3>Problem of Accelerating Data Deluge</h3>
<p>Why should we even consider this problem? Below are some reasons.</p>
<p>
  From storage point of view, we are generating a huge amount of data. As a result,
  we are running out of storage. In 2007, we generated more data than the total
  storage available. So now, we have no other option but to simply throw away some
  data.
</p>
<p>
  From sampling point of view, sampling rate calculated using Shannon-Nyquist theorem
  might be too high in many cases. It might be very expensive or simply impossible to
  design a sampling device for such rates.
</p>
<p>
  From communication point of view, increase in data generation rate is much higher than
  increase in transmission rate.
</p>
<br/>
<h3>Is there a solution?</h3>
<p>
  Yes. Compressive Sensing ! The main idea is to acquire far fewer samples of a signal in
  a signal independent fashion and then reconstruct the signal from these incomplete measurements.
</p>
<br/>
<h3>What is Compressive Sensing?</h3>
<p>
  As stated in an&nbsp;<a href="http://ieeexplore.ieee.org/xpl/articleDetails.jsp?tp=&amp;arnumber=
  4472240&amp;queryText%3Dan+introduction+to+compressive+sampling" target="_blank">article</a>
  written by Emmanuel J. Candes and Michael B. Wakin, Compressive sensing (CS) is a sensing/sampling
  paradigm that goes against the common knowledge in data acquisition. Conventional approaches
  to sampling follow Shannon-Nyquist theorem. Using CS theory, certain signals can be
  recovered from far fewer samples than traditional methods use.
</p>
