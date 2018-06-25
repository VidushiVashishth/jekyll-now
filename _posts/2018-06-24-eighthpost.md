---
layout: post
title: Babeltrace Demonstrations
---

Hi!

This post demostrates the conversion of rtems trace into Common Trace Format using babeltrace. The rtrace shell capability lets you print the fileio sample testcase's trace onto the console using `rtrace trace`. The blog post on trace buffering demonstrates how to generate these traces. The output of the instruction in the trace buffering demonstration blogpost is as follows:

{% highlight c %}

    0:00:26.703188295    2081911  0a010002 [  2/  2] > malloc((size_t) 00000130)
    0:00:26.703324403     136108  0a010002 [  2/  2] < malloc => (void*) 0x219be08
    0:00:26.703462953     138550  0a010002 [  2/  2] > malloc((size_t) 00000006)
    0:00:26.703597841     134888  0a010002 [  2/  2] < malloc => (void*) 0x219bf40
    0:00:26.703675966      78125  0a010002 [  2/  2] > malloc((size_t) 00000007)
    0:00:26.703810854     134888  0a010002 [  2/  2] < malloc => (void*) 0x219bf50
    0:00:26.703901186      90332  0a010002 [  2/  2] > malloc((size_t) 000003fc)
    0:00:26.704037904     136718  0a010002 [  2/  2] < malloc => (void*) 0x219bf60
    0:00:26.706615421    2577517  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.706758853     143432  0a010003 [200/200] < malloc => (void*) 0x219bf60
    0:00:26.707522404     763551  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 0000005d)
    0:00:26.707595036      72632  0a010003 [200/200] > malloc((size_t) 0000005d)
    0:00:26.707736027     140991  0a010003 [200/200] < malloc => (void*) 0x219bf60
    0:00:26.707815983      79956  0a010003 [200/200] < calloc => (void*) 0x219bf60
    0:00:26.708293888     477905  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.708437932     144044  0a010003 [200/200] < malloc => (void*) 0x219bfc8
    0:00:26.708658879     220947  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 00000080)
    0:00:26.708732121      73242  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.708876164     144043  0a010003 [200/200] < malloc => (void*) 0x219c050
    0:00:26.708956121      79957  0a010003 [200/200] < calloc => (void*) 0x219c050
    0:00:26.709034245      78124  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 00000080)
    0:00:26.709101384      67139  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.709245427     144043  0a010003 [200/200] < malloc => (void*) 0x219c0d8
    0:00:26.709325383      79956  0a010003 [200/200] < calloc => (void*) 0x219c0d8
    0:00:26.710109686     784303  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 00000061)
    0:00:26.710168279      58593  0a010003 [200/200] > malloc((size_t) 00000061)
    0:00:26.710300726     132447  0a010003 [200/200] < malloc => (void*) 0x219bfc8
    0:00:26.710375799      75073  0a010003 [200/200] < calloc => (void*) 0x219bfc8
    0:00:26.710861639     485840  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.711002630     140991  0a010003 [200/200] < malloc => (void*) 0x219c160
    0:00:26.711218695     216065  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 00000080)
    0:00:26.711287665      68970  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.711429877     142212  0a010003 [200/200] < malloc => (void*) 0x219c1e8
    0:00:26.711505560      75683  0a010003 [200/200] < calloc => (void*) 0x219c1e8
    0:00:26.711580634      75074  0a010003 [200/200] > calloc((size_t) 00000001, (size_t) 00000080)
    0:00:26.711644721      64087  0a010003 [200/200] > malloc((size_t) 00000080)
    0:00:26.711785712     140991  0a010003 [200/200] < malloc => (void*) 0x219c270
    0:00:26.711862616      76904  0a010003 [200/200] < calloc => (void*) 0x219c270
    0:00:26.712274604     411988  0a010003 [200/200] > malloc((size_t) 00000008)

{% endhighlight %} 

The `>` sign indicates entry into the function (whose name follows the symbol) and `<` indicates exit of the function. The first column displays the timestamp value. The second third and fourth columns indicate task id, task status related values. The entry instances of the functions is followed by a list of the type and value of the argument variables. Whereas the exit instances are followed by the type and value of the return variables.

Babeltrace comes with a set of standard plugins for taking input and output. These are shown below:

{% highlight c %}
$ babeltrace list-plugins

From the following plugin paths:

  - /home/vidushi/.local/lib/babeltrace/plugins
  - /usr/local/lib/babeltrace/plugins

Found 10 component classes in 4 plugins.

utils:
  Path: /usr/local/lib/babeltrace/plugins/babeltrace-plugin-utils.so
  Description: Graph utilities
  Author: Julien Desfossez, Jérémie Galarneau, Philippe Proulx
  License: MIT
  Component classes:
    'filter.utils.muxer': Sort notifications from multiple input ports to a single output port by time.
    'filter.utils.trimmer': Keep notifications that occur within a specific time range.
    'sink.utils.counter': Count notifications and print the results.
    'sink.utils.dummy': Consume notifications and discard them.

text:
  Path: /usr/local/lib/babeltrace/plugins/babeltrace-plugin-text.la
  Description: Plain text component classes
  Author: Julien Desfossez, Mathieu Desnoyers, Philippe Proulx
  License: MIT
  Component classes:
    'source.text.dmesg': Read a dmesg output from a file or from standard input.
    'sink.text.pretty': Pretty-print notifications (`text` format of Babeltrace 1).

ctf:
  Path: /usr/local/lib/babeltrace/plugins/babeltrace-plugin-ctf.so
  Description: CTF source and sink support
  Author: Julien Desfossez, Mathieu Desnoyers, Jérémie Galarneau, Philippe Proulx
  License: MIT
  Component classes:
    'source.ctf.lttng-live': Connect to an LTTng relay daemon and receive CTF streams.
    'sink.ctf.fs': Write CTF traces to the file system.
    'source.ctf.fs': Read CTF traces from the file system.

lttng-utils:
  Path: /usr/local/lib/babeltrace/plugins/babeltrace-plugin-lttng-utils.so
  Description: LTTng utilities
  Author: Julien Desfossez
  License: MIT
  Component classes:
    'filter.lttng-utils.debug-info': Augment compatible events with debugging information.

{% endhighlight %}

I saved the output of the rtems trace into a text file and ran the babeltrace conversion graph on it. The output of this command generated a ctf metadata file and a ctf stream. The command used considers babeltrace input to be a dmesg text input and declares ctf as the required output. It creates the relvant components and gives the values of the path for both the input and output traces.


{% highlight c %}

$ sudo babeltrace run --component=A:source.text.dmesg --params='no-extract-timestamp=no' --key=path --value ~/Desktop/babeltrace_exp2/Trace --component=B:sink.ctf.fs --key=path --value ./Desktop --connect=A:B

{% endhighlight %}

The output of this command creates a ctf stream in a file by the name of the input trace file. The output is as follows:

{% highlight c %}

vidushi@vidushi-HP-15-Notebook-PC:~/Desktop$ cd Trace
vidushi@vidushi-HP-15-Notebook-PC:~/Desktop/Trace$ sudo ls -alZ
total 16
drwxr-x--x 2 root    root    ? 4096 Jun 24 12:01 .
drwxr-xr-x 7 vidushi vidushi ? 4096 Jun 24 12:01 ..
-rw-r----- 1 root    root    ?  688 Jun 24 12:01 metadata
-rw-r----- 1 root    root    ?   71 Jun 24 12:01 stream-0-0
vidushi@vidushi-HP-15-Notebook-PC:~/Desktop/Trace$ 

{% endhighlight %}
