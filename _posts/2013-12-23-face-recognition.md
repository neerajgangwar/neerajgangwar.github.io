---
layout: post
title: Face Recognition
date: 2013-12-23 21:36:00
author: Neeraj Gangwar
tags:
- pattern classification
- face recognition
- eigenfaces
- classification
- computer vision
modified_time: '2014-03-24T07:25:14.932-07:00'
thumbnail: http://4.bp.blogspot.com/-qgqly2LpSUM/UrkWmOMu2cI/AAAAAAAAAd4/-_5cGhzalZM/s72-c/Screenshot+from+2013-12-24+10:36:55.png
blogger_id: tag:blogger.com,1999:blog-8995384349089948093.post-2653147304107158034
blogger_orig_url: http://neerajgangwar.blogspot.com/2013/12/face-recognition.html
---

<p>
  This post is not going to be about compressive sensing or sparse representation. I have
  been trying to find a field where I can use either of these approaches. I read that sparse
  representation can be used in face recognition. Well, I am still not sure. I am working on
  its validity. In the meantime, I tried to implement basic face recognition system.
</p>

<p>
  I read <a href="http://www.face-rec.org/algorithms/PCA/jcn.pdf" target="_blank">Eigenfaces
  for Recognition</a>&nbsp;by Matthew Turk and Alex Pentland. It's a very basic face recognition
  approach.
</p>

<p>
  There are two modules. First module is to train the system. It takes a set of faces and
  generates the features (called eigenfaces) and weight vectors (or projections). The second
  module is for face matching. It uses these features to calculate the weight vectors for a
  new image and decides the face class for this new face.
</p>

<br/>
<h4>Approach</h4>
<ol>
  <li>
    Principal Component Analysis (PCA) is used in this approach to reduce the dimensionality of
    the data. <a href="http://www.stat.cmu.edu/~cshalizi/490/pca/pca-handout.pdf" target="_blank">
    Here</a> is a good tutorial on PCA.
  </li>
  <li>
    Suppose we have \(M\) images of size \( N \times N \). These images are reshaped into a
    \( N^2 \times 1 \) vectors \( \Gamma_1, \Gamma_2 \dots \Gamma_M\). Now our aim is to summarize
    them by projecting into a \(M'\) dimensional subspace \( M' \le M \).
  </li>
  <li>
    PCA gives an orthonormal basis for \( M \) dimensional subspace \( \mathbf{u}_k \). This basis
    is named as eigenfaces.
  </li>
  <li>
    Original images are transformed into its eigenfaces components (weights) using
    $$ \omega_k = &nbsp;\mathbf{u}_k( \Gamma - \Psi) $$ where $$ \Psi = \frac{1}{M} \sum_{n=1}^{M} \; \Gamma_n$$
    The reason to introduce \( \Psi \) is to make the data zero mean. More details are given in the tutorial
    mentioned above.
  </li>
  <li>
    A face class (or weight vectors) is defined as \( \Omega = [\omega_1, \omega_2 \dots \omega_M] \) for
    each individual face. We generally take average weight of these weight vectors for different images
    of the same face to define a face class.
  </li>
</ol>

<br/>
<h4>Few original faces</h4>
<p>I used 400 images. 10 per person.</p>
<div style="text-align: center;">
  <img border="0" src="http://4.bp.blogspot.com/-qgqly2LpSUM/UrkWmOMu2cI/AAAAAAAAAd4/-_5cGhzalZM/s640/Screenshot+from+2013-12-24+10:36:55.png" height="416" width="640" />
</div>

<br/>
<h4>Few Eigenfaces</h4>
<p>
  Actually there are 400 of them since I used 400 images. But I used only 200 corresponding to
  200 largest eigenvalues (sufficient to represent original images with tolerable amount of error).
</p>
<div style="text-align: center;">
  <img border="0" src="http://4.bp.blogspot.com/-py9JWa3nefE/UrkVtzvYWjI/AAAAAAAAAdw/QKeXDciGiSM/s640/Screenshot+from+2013-12-24+10:32:08.png" height="417" width="640" />
</div>

<br/>
<h4>Face Matching</h4>
<p>
  It's really simple. We just have to calculate weight vector for the new face and compare with those
  of original images to find out the closest face. This way we find out the face class for a new face.
  An error threshold is also defined to ensure that the new image does belong to a particular face class
  in addition to being closest to it.
</p>

<br/>
<h4>Testing</h4>
<p>
  I used first 9 images of each individual face to train the system i.e. 360 images. I gave 10th face as
  input to face matching. See the plot for error (minimum value of error corresponds to closest face class)
</p>

<br/>
<h5>For 10th image of 1st face class</h5>
<div style="text-align: center;">
  <img border="0" src="http://4.bp.blogspot.com/-tZp_KpQkGwY/Urkb6oLchrI/AAAAAAAAAeI/1deNtIUvoKQ/s640/Screenshot+from+2013-12-24+10:58:28.png" height="499" width="640" />
</div>
<p>It can be seen that height of first bar is minimum.</p>

<br/>
<h5>For 10th image of 8th face class</h5>
<div style="text-align: center;">
  <img border="0" src="http://1.bp.blogspot.com/-vOp4MEcp4IQ/Urkb8pY1TXI/AAAAAAAAAeQ/cvstEQYUBTI/s640/Screenshot+from+2013-12-24+10%253A58%253A41.png" height="488" width="640" />
</div>
<p>Here height of 8th bar is minimum corresponding to minimum error.</p>

<br/>
<p>
  My code can be found <a href="https://github.com/neerajgangwar/Face-Recognition" target="_blank">here</a>.
  Code is not structured but it'll give you a basic idea about how to write your own.
</p>

<p>
  Face database is available <a href="http://www.cl.cam.ac.uk/research/dtg/attarchive/facedatabase.html" target="_blank">
  here</a>.
</p>

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
