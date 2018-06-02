---
layout: post
title: Common Trace Format(CTF) trace generation in RTEMS
---
Rtems tracing framework comprises of the following main components:
1) Rtems trace linker (https://github.com/VidushiVashishth/rtems-tools/tree/master/linkers)
2) Capture Engine

The RTEMS Trace Linker is a post link tool that performs a re-link of user's application to produce a trace executable. A trace executable has been instrumented by the RTEMS Trace Linker with additional code that implements software tracing. A key requirement of the trace process in RTEMS is to take existing code in a compiled format (ELF) and instrument it without rebuilding that code from source and without annotating that source with trace code. The trace linker is controlled using configuration files in a flexible manner and can be configured to follow various tracing schemes. There are three types of configuration files:

1) <em>User configuration</em>- This is the configuration input into the trace linker for defining the values of trace generator, traces, triggers and enables for the user application to be traced.
2) <em>Generator configuration</em>- This is used to encapsulate a specific method of tracing. Rtems currently provides generators for trace buffering, printk and printf. 
3) <em>Tracer configuration</em>- This is a library of common base trace configurations used by an application.


Currently trace buffering can be accomplished using two schemes:
1) Rtems trace using trace buffering
2) Rtems trace using printk 

My aim is to develop Rtems trace using Common Trace Format (CTF). CTF brings a range of features which rtems can benefit from. Before I explain how this will be acomplished it will be beneficial if the reader understands Rtems trace using trace buffering since several steps of the workflow will be common in both schemes. However if you are an impatient reader feel free to skip the next bit!

##Rtems trace using trace buffering

This scheme of tracing goes through the flow of events described in the follwing image. 

Step 1: The user creates an application and user configuration file. The configuration file specifies the use of the trace buffer generator and other standard initializations. The user then invokes the trace linker using a configuration command and a link command to link the application executable. The trace linker uses the application files in compiled format (ELF) and the libraries used to build the application for performing this link. 

Step 2: The RTEMS Trace Linker reads the user's configuration file and that results in it reading the standard Trace Buffering Configuration files installed with the RTEMS Trace Linker. The trace linker uses the target compiler and linker to create the trace enabled application executable. It wraps the functions defined in the user's configuration with code that captures trace records into the statically allocated buffer. The trace wrapper code is compiled with the target compiler and the resulting ELF object file is added to the standard link command line used to link the application and the application is re-linked using the wrapping option of the GNU linker.  

Step 3: The trace linker creates an executable which is capable of running on the target hardware or simulator.

Step 4: RTEMS shell provides the "rtrace" command to display and save trace buffers. 

![_config.yml]({{ https://vidushivashishth.github.io/ }}/images/img2.png)
My next blog details an example of tracing the fileio sample testcase with trace buffering. Check it out!

##Rtems trace using CTF

The procedure for CTF conversion has been mentioned in the previous blogpost. The flowchart is added here again for easier reference. The steps 1, 2 and 3 are similar to the trace buffeering scheme with minute differences

Step 1: The user creates an application and user configuration file. The configuration file specifies the use of CTF generator and other standard initializations. The user then invokes the trace linker using a configuration command and a link command to link the application executable. The trace linker uses the application files in compiled format (ELF) and the libraries used to build the application for performing this link. 

Step 2: The RTEMS Trace Linker reads the user's configuration file and that results in it reading the standard CTF Trace Configuration files installed with the RTEMS Trace Linker. The trace linker uses the target compiler and linker to create the trace enabled application executable. It wraps the functions defined in the user's configuration with code that generates trace records for the Capture Engine. Each trace record is given an identifier that is used to decode the trace data. The trace wrapper code is compiled with the target compiler and the resulting ELF object file is added to the standard link command line used to link the application and the application is re-linked using the wrapping option of the GNU linker. 

Step 3: The trace linker creates an RTEMS ELF executable capable of running on the target hardware or simulator. It also creates a CTF configuration file defining the trace records and the format of the trace records. 

Step 4: The user runs the application in the GNU debugger and loads the RTEMS Tools Project's Python support code. This code contains support to manage the capture engine and extract the data from the target 

Step 5: The RTEMS Trace data and the CTF configuration is passed to Babletrace where it is converted to a CTF format data file. CTF data files can be viewed with a range of visualization tools. 


![_config.yml]({{ https://vidushivashishth.github.io/ }}/images/img1.png)


Currently the trace linker does not have the standard CTF Trace Configuration files. I am working on their development. 


