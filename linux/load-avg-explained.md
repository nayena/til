# Load Average Explained

This is an excerpt from an [amazing article](http://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html) from the legendary Brendan
Gregg, that works at Netflix.

A lot of people I've talked to over the years seem to think that the load
avg is some sort of golden magic number, that shows how busy a system is.

While that is _somewhat_ true, there's more to it than just that.

If you look at the linux source code for [loadavg.c](https://github.com/torvalds/linux/blob/master/kernel/sched/loadavg.c),
there is a comment which states it quite well:

```c
/*
 * kernel/sched/loadavg.c
 *
 * This file contains the magic bits required to compute the global loadavg
 * figure. Its a silly number but people think its important. We go through
 * great pains to make it work on big machines and tickless kernels.
 */
```

So what does Gregg think we should look at? Load Avg only indicates that
the system is getting busier. To find out _why_ it is busier, look at:

* CPU metrics, like per-CPU utilization, scheduler latency and
CPU run queue
* Disk metrics
* Lock Metrics