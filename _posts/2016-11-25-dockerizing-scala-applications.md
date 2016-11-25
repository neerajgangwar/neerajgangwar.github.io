---
layout: post
title: Dockerizing Scala Applications
comments: true
author: Neeraj Gangwar
tags:
- docker
- scala
- dockerize
- play
- scala application
---

If you are in doubt about using Docker, Google Trends says enough about
its popularity.

<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/815_RC05/embed_loader.js"></script>
<script type="text/javascript">
  trends.embed.renderExploreWidget("TIMESERIES", {
      "comparisonItem":[{"keyword":"/m/0wkcjgj","geo":"","time":"today 5-y"}],
      "category":0,
      "property":""
    }, {"exploreQuery":"q=%2Fm%2F0wkcjgj","guestPath":"https://www.google.co.in:443/trends/embed/"});
</script>

<br/>
Until a few release ago, it was quite a hassle to run Docker. However it has
become stable lately. I have been using Docker for 1.5 years and my experience
has been quite amazing. I love how I can run a Docker image built on my
laptop in production without any headache. Also, it's very easy to distribute a
runnable image or run someone else's image on your machine. You need Docker
insatlled on your system and you can just run the application.

In this post, we are going to create Docker image for a simple Scala application.

<br/>
<h4>What is Docker?</h4>
Docker is a tool designed to run applications in an isolated environment using
containers. Containerization is an OS level virtualization method to run
multiple systems on a single kernel without launching an entire virtual
machine. On Linux based operating systems, it is achieved using [LXC (Linux
Containers)](https://en.wikipedia.org/wiki/LXC). Docker uses built-in Linux
containment features like CGroups, Namespaces, UnionFS, chroot to run applications
in the virtual environment.

LXC is an API for Linux containment features. Initially, Docker was built on top
of LXC. Starting with Docker 0.9, it has been replaced with `libcontainer`.

To learn more about Docker, [official documentation](https://docs.docker.com/engine/understanding-docker/)
is a good place to start.

<br/>
<h4>Docker with Scala Build</h4>
To create Docker image for a Scala application, we'll use `sbt-native-packager`.
<blockquote>
  sbt-native-packager focuses on creating a Docker image which can “just run”
  the application built by SBT.
</blockquote>

To achieve this, a few entries have to added in `settings` of project definition.

Let's create Docker image for a simple service. Here is one that uses [Akka
HTTP](http://doc.akka.io/docs/akka-http/current/scala.html). I just picked the
first example I saw in Akka HTTP documentation. This service listens on `8080`
and displays a simple hello page for `/hello` path. Full code is available on
[GitHub](https://github.com/neerajgangwar/dockerize-scala-app).
<script src="https://gist.github.com/neerajgangwar/8a3e9062e86aa6371e193a3ae6866bc4.js?file=ExampleService.scala"></script>

To build Docker image for this service, project definition in `Build.scala` looks
like
<script src="https://gist.github.com/neerajgangwar/8a3e9062e86aa6371e193a3ae6866bc4.js?file=Build.scala"></script>

L15 specifies the main class of the project.

L16 specifies a list of TCP ports to expose from the Docker image. Your application
must be listening on one of these ports.

L17 specifies the entry point for Docker. This command will be executed when
Docker container is run.

L18 specifies the repository to which the image should be pushed when `docker:publish`
is run.

L19 specifies the base image to be used when building Docker image for this
project.

Run `docker:publishLocal` after compilation to publish the Docker image locally.

<br/>
<h4>Docker with Play Application</h4>
With Play framework, project definitions are written in `build.sbt` by default.
Here, `root` or `main` project definition looks like
<script src="https://gist.github.com/neerajgangwar/8a3e9062e86aa6371e193a3ae6866bc4.js?file=build.sbt"></script>

L15 prevents Docker from creating images for the subprojects separately.

<br/>
<h4>Build your own Baseimage</h4>
I personally prefer [`phusion/baseimage`](http://phusion.github.io/baseimage-docker/)
which is a minimal Ubuntu baseimage. In a lot of cases, it so happens that you need some
packages to run your applications. Rather than adding the code to install these
packages in all build files separately, it's easier to build an image containing all the
packages and use it as baseimage for your projects.

For example, if you need Java installed, you can create a Dockerfile with following
code and build your own baseimage.
<script src="https://gist.github.com/neerajgangwar/8a3e9062e86aa6371e193a3ae6866bc4.js?file=Dockerfile"></script>

<br/>
<h4>Resources</h4>
<ol>
  <li>Example Code: <a href="https://github.com/neerajgangwar/dockerize-scala-app">
    https://github.com/neerajgangwar/dockerize-scala-app
  </a></li>
  <li>Official Docs: <a href="https://docs.docker.com">https://docs.docker.com</a></li>
  <li>sbt-native-packager: <a href="http://www.scala-sbt.org/sbt-native-packager/formats/docker.html">
    http://www.scala-sbt.org/sbt-native-packager/formats/docker.html
    </a>
  </li>
</ol>
