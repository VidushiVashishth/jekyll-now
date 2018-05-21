---
layout: post
title: Use cases for the RTEMS tracing system 
---

This post describes the application of the RTEMS trace framework and why anyone developing applications for rtems may find it useful. 

## Why would anyone require tracing?

Real time applications follow strict time and performance constraints. Tracing can prove to be an efficient debugging tool which can help analyze the performance of such applications and identify tricky bugs. Some common issues with real time applications for which rtems tracing framework will prove to be useful are modeled as use cases as follows:

1) <em>Detecting deadlock conditions:</em> Real time applications inherently involve parallel execution of tasks to provide faster computation and results. However there might exist cases when these tasks compete for common resources (eg processor time) and enter a circular unending wait. The performance of an application under deadlock deteriorates quickly. Hence such conditions must be detetcted timely so that the application programmer can take the necessary steps to avert deadlocks.

2) <em>Track complex thread operations including entry and exit events:</em> Threads are primarily useful in introducing concurrency of task execution and hence can be a vital component to real time applications. However using threads in an application is a complex and convoluted task. Tracing framework can prove to be an efficient programming tool to track down problems with thread execution. Tracing thread creation, deletion, entry and exit operations can be considered as an important use case in this regard.

3) <em>Tracing a function:</em> This is a basic tracing paradigm which can be useful in case of numerous nested calls to different functions.

4) <em>Tracing user extensions:</em> Tracing user defined extensions in rtems applications can also be considered as an importan use case for the rtems tracing framework.

5) <em>Detecting race conditions:</em> Another common issue with parallel execution or concurrent processing is encountering race conditions. This scenario usually occurs when multiple processes enter common critical sections or access and update common data structures. There is no way to identify which process's modification will be reflected as the final output. By tracing processes' execution as well as values of common data structures we might be able to analyse the existence of race conditions.

## What is babletrace and CTF?

In order to implement these use cases with the tracing framework we must understand the underlying technologies used along with it. The two main components crucial to realization of tracing are the Common Trace Format (CTF) and babletrace. 

CTF is a binary trace format which is flexible and fast to write. A typical CTF trace comprizes of numerous streams of binary events. One can decide the number of streams into which the binary events can be divided into. However the metadata stream which consists of data about the about the trace is mandatory. Every binary stream consists of several packets which contain the trace information or payload. The organization of these packets follows an orderly and maybe padded sequence of events. Trace Stream Description Language (TSDL) is a descriptive language which is used to describe all the header, contexts and event fields of a CTF trace. 

Babeltrace is an open source trace format converter as well as the reference parser implementation of the Common Trace Format. It was originally used to parse CTF traces produced by LTTng and produce readable text output. LTTng is the open source tracing framework for Linux operating system. 

## Why do I need these, and what can I do with CTF and babletrace?

The variety of CTF's features, its applicability to rtems and the post processing and real time monitoring tools available with it are valuable. Hence CTF integration with rtems tracing framework will prove to be a productive investment as a project. The use of babeltrace would enhance the debugging process for rtems applications by many folds. It would be easier for users to understand the problems encountered by their application code bases, since babeltrace provides this in human readable text output. 

The current tracing framework does not generate CTF traces directly. The procedure to generate CTF traces using babletrace follows the steps of the given diagram:

![_config.yml]({{ https://vidushivashishth.github.io/ }}/images/img1.png)

The next couple of blogs would center around implementing the aforementioned use cases. These developments would also eventually be documented in the RTEM's user's manual as a new chapter on tracing. An example of CTF trace conversion using the procedure in the above flowchart will be documented in the subsequent blogposts.

## References

http://diamon.org/babeltrace/

http://diamon.org/ctf/#ctf-in-a-nutshell

https://docs.rtems.org/branches/master/user/index.html
