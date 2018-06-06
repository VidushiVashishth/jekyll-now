---
layout: post
title: CTF Integration approaches comparison
---

Hi!

At present there are two approaches that I can follow to deliver CTF traces for user applications in RTEMS:

# 1) Using Babeltrace

As seen from previous posts RTEMS currently supports trace outputs in the form of:

- Printing output on console
- Buffer contents as binary files saved to disk.

Babeltrace [1] is a trace conversion tool which primarily converts CTF inputs into other formats (text, ctf metadata, dummy and Lttng live). However the babeltrace parser can be made to take RTEMS current trace output format as input and return the converted traces in CTF. The problems with this approach pertain to sustainability. This is not likely to be a long term solution as it is dependent on the conversion formula involved. Any changes in the output format will lead to reworking of this entire approach. However it is likely to be delivered faster :p

# 2) Integrating barectf

barectf [2] is a command-line generator of ANSI C tracers which output Common Trace Format packets natively. It takes a yaml configuration file as input and generates CTF metadata as well as c code of tracing functions. The application then uses these tracing functions in its source and generates binary streams of CTF. To use barectf with RTEMS the following approach will need to be followed:

1) Writing a yaml configuration deciding the structure of the CTF streams to be generated. This will be input to barectf to generate CTF metadata and tracing code. 
2) Creating a Trace Linker Generator which guides the Trace Linker to use the barectf generated functions in the wrapper of the application to be traced. The final wrapper code will then be compiled and the application executable will be relinked.
3) On execution of the application CTF traces will be generated. These could be converted into human readable form using babeltrace and CTF metadata generated in first step.

It might seem to the reader that since barectf requires change in source code of application it conflicts with rtems trace requirements (of not touching application source). However this approach makes changes to the wrapper code instead of the source code and hence keeps the rtems trace requirements in tact. The problems with this approach lie in deciding a suitable yaml configuration for the process. This is under discussion. 

# References

[1] http://diamon.org/babeltrace/

[2] https://github.com/efficios/barectf








