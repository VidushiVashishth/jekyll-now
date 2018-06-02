---
layout: post
title: Trace Buffering Demonstration
---

This post demonstrates trace buffering for the fileio sample testcase.

# Prerequisites:

1) Setup Rtems for the sparc/erc32 architecture bsp pair. Refer to the quickstart section of rtems user manual documentation[1].

2) Download the fileio configuration file[2] and store it on the top of the installed bsp's directory.

3) Change the value of the keys: rtems-path and prefix according to your rtems installation. The rtems-path is the path to the bsp installation and prefix is the path to the tools used to build rtems. 

# Demo:

cd into the directory where you've downloaded the configuration file and run the following commands to generate traces:

BSP is configured with the following command.

{% highlight c %}

../rtems/configure --target=sparc-rtems5 --prefix=/development/rtems/5 --enable-networking --enable-tests --enable-rtemsbsp=erc32 --enable-cxx

{% endhighlight %}

The next two commands are used to link the fileio executable.The -B option signifies the use of the complete path to the required directory or file. Confirm the path of the fileio's executable and object files in the last line of the command.

{% highlight c %}

sparc-rtems5-gcc -B/home/vidushi/development/rtems/5/sparc-rtems5/erc32/lib/ -specs bsp_specs -qrtems -mcpu=cypress -O2 -g -ffunction-sections -fdata-sections -Wall -Wmissing-prototypes -Wimplicit-function-declaration -Wstrict-prototypes -Wnested-externs -Wl,--gc-sections -mcpu=cypress -o sparc-rtems5/c/erc32/testsuites/samples/fileio.exe sparc-rtems5/c/erc32/testsuites/samples/fileio/fileio-init.o

{% endhighlight %}

This is the trace linker command to generate and compile the wrapper c file for the application. The link command follows the escape sequence "--". "-C" option denotes the name of the user configuration file and "-W" specifies the name of the wrapper c file. 

{% highlight c %}

rtems-tld -C fileio-trace.ini -W fileio-wrapper -- -B/home/vidushi/development/rtems/5/sparc-rtems5/erc32/lib/ -specs bsp_specs -qrtems -mcpu=cypress -O2 -g -ffunction-sections -fdata-sections -Wall -Wmissing-prototypes -Wimplicit-function-declaration -Wstrict-prototypes -Wnested-externs -Wl,--gc-sections -mcpu=cypress -o sparc-rtems5/c/erc32/testsuites/samples/fileio.exe sparc-rtems5/c/erc32/testsuites/samples/fileio/fileio-init.o

{% endhighlight %}

The following command is used to run the application. Hit enter key quickly and type "s" and "root" and "pwd" to run the rtems shell. Use the rtrace staus, rtrace trace and rtrace save commands to know the status of the tracing, display the contents of the trace buffer and save the buffer to disk. 
 
{% highlight c %}

sparc-rtems5-run sparc-rtems5/c/erc32/testsuites/samples/fileio.exe

{% endhighlight %}

# Relevant Commits:
https://github.com/RTEMS/rtems-tools/commit/14a1c0978d4dc1f29f5568efb075524544066874

# References:

[1] https://docs.rtems.org/branches/master/user/start/index.html

[2] https://devel.rtems.org/attachment/wiki/Developer/Tracing/Trace_Buffering/fileio-trace.ini

