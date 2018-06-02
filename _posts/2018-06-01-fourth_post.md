---
layout: post
title: CTF trace generation
---
Rtems tracing framework comprises of the following main components:
1) Rtems trace linker (https://github.com/VidushiVashishth/rtems-tools/tree/master/linkers)
2) Capture Engine

The RTEMS Trace Linker is a post link tool that performs a re-link of your application to produce a trace executable. 
A trace executable has been instrumented by the RTEMS Trace Linker with additional code that implements software tracing. 
A key requirement of the trace process in RTEMS is to take existing code in a compiled format (ELF) and instrument it without rebuilding that code from source and without annotating that source with trace code.
The trace linker is controlled using configuration files in a flexible manner and can be configured to follow various tracing schemes:

The RTEMS Trace Linker takes the ELF files and libraries used to build a executable and performs a link that instruments the code so the functions a user is interested in generate suitable trace records.
The user provides a configuration file that defines the trace set up. The instrumented code logs the trace data using the Capture Engine. 
This is a thread and SMP safe software module that takes trace records and filters the data in real-time based on trace enable records and triggers. 
A triggered system will log into buffers trace records that are enabled. The Capture Engine maintains a trace buffer per CPU to avoid locking overheads in SMP applications.

Currently trace buffering can be accomplished using two methods:
1) Rtems trace using trace buffering
2) Rtems trace using printk
