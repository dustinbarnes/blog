---
id: articles.software-performance-101
title: Software Performance 101
desc: ''
updated: 1682484383443
created: 1396500000000
---
> Note: This article was written in 2014, and the tools/techniques may have changed. 

# Terms

We are going to discuss how to measure, analyze, diagnose, and ultimately solve performance problems. First, however, we must define what we mean by a "performance problem." There are three major items we talk about when we discuss performance. However, as we all know, that's only part of the picture.

## Latency and Throughput

When we think about performance problems, latency and throughput are the two measures that are immediately apparent.

Latency is the time it takes to complete a single operation. Simply put, it's the time taken for some action to complete. Gamers often see this represented as their "ping time." When a webpage is slow to load, that's high latency to the user. A new phone feels faster because the UI is more responsive -- the latency of operations is lower.

Throughput is the number of work items that can be completed in a unit of time. A work item can be anything -- production of a motorcycle, completion of a REST call, or the download of a packet of data. In the world we are talking about, the most common measure we see is requests per second. Essentially, "how many users can my site support at once?"

### Intertwined

While the measures above are separate in their own way, they are often interwined quite tightly. Imagine a doctor's office, where each visit takes 15 minutes. If there is one doctor, and nobody in the waiting room, you will be seen immediately. If there are 4 people in front of you, you will be seen in an hour. Your perceieved latency is 1 hour. The throughput is 4 per hour. If the office adds another doctor, your perceieved latency is 30 minutes, and the office's throughput is 8 per hour. By increasing the throughput, we have decreased the latency.

## Congestion

We just learned that by increasing throughput, we can decrease latency. However, that's not quite enough to describe performance problems. The latency above was a fixed number -- 15 minutes. What if the latency was more variable? Some patients may take 5 minutes, other patients may take 2 hours. Additionally, imagine this office has 40 people in it. Your ability to see a doctor is delayed because of the number of people in front of you. This is refered to as congestion (or, if you want to be more computer science-y, queueing delay).

## All Together Now

Let's bring this back to what most of us do: serve webpages. Let's start very simple, with a static HTML page and a server that can handle one request at a time. A single-threaded server handles a request, and returns a page. With little traffic, the latency is quite predictable. When we get more traffic, we start to experience congestion. The average latency to the user increases, and they become unhappy.

To combat that increase in traffic, we add another thread. We can now handle two requests at a time. With low enough traffic, requests are served immediately. However, the throughput increase is not exactly doubled. There is some overhead to managing threads, dispatching requests, context swaps by the CPU, number of cores, and more. For each increase in parallelization, we get a diminished return in throughput and latency. In fact, it's possible to create so many threads that the system is overwhelmed managing them all, and both throughput and latency are increased!

This can be further complicated, of course. Imagine a shared resource, like a database connection. In order for the software to work correctly, only one thread at a time is allowed to query and receive data from the database. This introduces yet another possible source of congestion, even if there is no congestion connecting to the web server. These shared resources must be carefully balanced with the rest of the system to optimize performance.

## Debrief

Tuning a system for maximum performance is the art of optimizing all three of these parameters with regards to your system. Maybe you don't care about congestion, just that requests are served as quickly as possible once processing starts. Maybe you don't care about latency, and just want raw throughput. Maybe you want no congestion whatsoever.

# Measuring

There is an old joke in the programming world regarding optimization:

> The First Rule of Program Optimization: Don't do it. The Second Rule of Program Optimization (for experts only!): Don't do it yet.

Optimizing software is tricky business. One thing I've learned over the years is that the problem is almost never where you think it is.

## Math

To get started, we need to have a quick math refresher. Say that you have an application that reads a REST request, makes some backend calls, and returns a JSON response.

The amount you gain from optimization is how much the optimized chunk of code is used in the overall lifecycle. If one backend call is responsible for 8% of your runtime, and you are able to 100% optimize it (reduce the time to 0), your overall metric is only seeing an 8% increase.

In our 8% example, the more likely optimization is somewhere around 20%. Therefore, if we look at it as a function of the total time, we have saved 1.6% on runtime. Imagine a different function is responsible for 20% of your runtime, but you can only optimize it by 10%. This actually works out to be better, as this now speeds up your application by 2%.

What you really want to do is identify the biggest pain points, and tackle those first. But, how do you find those?

## Average, Average, and Average

We all know the term "average," but we need to be more specific when it comes to analysis. Let's say we have a collection of 20 numbers. Each individual number is called a sample.

If you remember your math, there are 3 things that could mean average: mean, median, and mode. Mode is not generally useful in this scenario. Mean and median are. Mean is what most people are referring to when they say average. The mean is simply adding all the samples up, and dividing by the number of samples. The median is the sample where, if you sorted the numbers, is right in the middle. If your distribution of samples is perfect (like a bell curve), the mean and median should be quite close. When they are not, you have a skewed distribution.

## Percentiles

Averages give you a decent idea of what is happening, but it doesn't tell the whole story. Imagine I told you that your mean webpage response time was 500ms. Seems ok, right? What if we then said that 40% of the people were under 100ms, 40% were under 500ms, and 20% were over 2 seconds. The mean stays the same. This means that 20% of your users have an absolutely terrible experience. We call this a percentile. In much the same way we measure uptimes, there are a few key percentiles we will care about: 95%, 99%, and 99.9%. Most often in a percentile distribution, we are showing the count of items in the set that, when sorted, fit within the percentile bounds.
Standard Deviation

This should be the last bit of math. When you take the mean of a set of numbers, you can also calculate a standard deviation. This is the mean distance from the mean for all samples in the set. The smaller the number, the more concentrated your samples. The larger the number, the more variability you have.

## Tools of the Trade

Performance analysis and optimization is a bit of a blended art and science. The art comes from optimizing the code. The science is how we find trouble spots. In the traditional fashion of science, we want to create a repeatable test. We will run this test frequently, making small tweaks in the code under test, and see how they affect the performance. The key principles in a good test suite are isolation and consistency. Given the same code base, you expect performance results to be comparable between runs.

Now that we know this, and we have our math refreshed, let's talk about some tools we use when evaluating performance.

### Gatling

Gatling is one of our favorite tools for performance analysis. It is a high-performance load testing tool, with very pretty (read: management friendly) data visualizations. It is especially good for testing REST-based services. A typical result would look like this:

There are additional graphs and more fine-grained reporting available.

Gatling can also be set up to run as part of your CI server! It has a simple command-line interface, and the project can be maven-ized with this maven plugin. A continuous performance test of your application in a CI environment is fantastic for Continuous Delivery.

We use this largely as a replacement for JMeter. JMeter is significantly more difficult to get started in, and it's difficult to use without the GUI. Gatling is very well situated for headless operation.

### Yammer Metrics

Ok, you have measured with Gatling and you've found something you don't like. How do we take it down to the next level? Yammer Metrics is our best friend for JVM-based applications. This is an AOP-based package that exposes several annotations. Our favorite annotation is `@Timed`, which is a combination of `@Metered` and `@Histogram`. From the documentation for `@Metered`:

> Meters measure the rate of the events in a few different ways. The mean rate is the average rate of events. It’s generally useful for trivia, but as it represents the total rate for your application’s entire lifetime (e.g., the total number of requests handled, divided by the number of seconds the process has been running), it doesn’t offer a sense of recency. Luckily, meters also record three different exponentially-weighted moving average rates: the 1-, 5-, and 15-minute moving averages.

From the documentation for `@Histogram`:

> Histogram metrics allow you to measure not just easy things like the min, mean, max, and standard deviation of values, but also quantiles like the median or 95th percentile.

> Traditionally, the way the median (or any other quantile) is calculated is to take the entire data set, sort it, and take the value in the middle (or 1% from the end, for the 99th percentile). This works for small data sets, or batch processing systems, but not for high-throughput, low-latency services.

> The solution for this is to sample the data as it goes through. By maintaining a small, manageable reservoir which is statistically representative of the data stream as a whole, we can quickly and easily calculate quantiles which are valid approximations of the actual quantiles. This technique is called reservoir sampling.

The best part about these annotations is that they can be in your app at all times. There are multiple ways to get the data out. The normal favorites (JMX, Servlet) are there. In addition, it can also stream data to Ganglia or Graphite to fit into an existing performance monitoring package.

### YourKit

> Note: YourKit is paid, commercial software. While we generally try to recommend free/libre software, this tool is so much better than the OSS alternatives that we believe it is worth the money.

YourKit is a Java profiler. It runs a small agent in your VM, to which a GUI tool connects. The profiler incurs no performance penalty when it is not profiling the application, so it is safe to leave the JAR file in place in production.

The profiler has 2 ways of measuring: sampling and tracing. With sampling, the tool grabs a snapshot of stacktraces every few seconds, and determines some metrics. This is a very low-overhead method of gathering data.

Tracing is significantly more expensive. It will slow your VM by an order of magnitude, but its method tracing is perfect. This is where you want to go to get a deep dive on a method.

So what do you get for all of this? There's actually too much to talk about in this post. You get memory telemetry (how quickly objects are created and destroyed, what types of objects they are, and where they're created from). You get CPU telemetry, including charts on wall time, your process' time, and time spent in GC, all graphed in realtime. This also includes call stack telemetry. It looks at the path from the beginning of the action all the way down to each individual method. It counts the number of invocations, and tracks telemetry on the rate and latency. This is a fantastic way to identify which processes are taking the longest. In fact, let's look at a screenshot of this method telemetry:

This is clearly a Spring/Hiberate app interacting with a database. You can see the invocation count, but you can also see the percentages. Those percentages represent the proportion of total time that the method has taken. Luckly, YourKit has also abstracted this into another pane, the Hotspot Identifier. This is basically a no-brains way of determining which methods are taking the most time. This, as we discussed early in the article, will give us the best bang for our optimizing buck.

## The Observer Effect

This is an axiom in physics that states you cannot measure a system without changing it. This is also true with all three tools above.

Gatling can consume a lot of resources. It's a bad idea to have both Gatling and the application under test on the same machine. It's also a bad idea to have Gatling on a machine that experiences variable load, as it can throw off your numbers. Ideally, you have a dedicated load testing machine to drive load.

Yammer Metrics has been quite minimal performance-wise, but it does incur some minor overhead. We happen to think that it's worth the small overhead for the insights it gives us, but that's a decision ultimately to be made by your engineering team.

YourKit hooks right into the VM. It incurs the overhead of measuring and monitoring all of the data an reporting it back to the tool. While this tool is mostly useful for tracking down a known performance issue, it can be fun to use it on your application to see if there are any easy wins performance-wise.

## Tying it All Together

We now have 3 tools, and they work together for us. Gatling gives us a repeatable test runner. It can generate load on-demand at any time. We get a high-level picture of the end-user's experience. Yammer Metrics keeps real-time metrics data on all instrumented methods, and is exposed in a consumable interface. You can even hook up your favorite monitoring system to alert if performance drops. Finally, YourKit gives you the deep insight required to identify the trouble spots in your code base. Once you make a change that looks better in YourKit, you can test it with Gatling to confirm that it makes the end-user experience better, and use Yammer Metrics to confirm the ongoing performance.

Now that we have the tools for measuring  issues, the next step is identifying and fixing the issues, which will be the subject of the next post in this series.

# Causes of Bad Performance

In the previous paragraphs, we talked about some language and concepts, and then we got into tools and some more math. This has allowed us to identify places we should spend our time.

## Why?

The first question we ask ourselves when we're tasked with performance enhancements is why? There are a lot of factors to consider, however, there are some basic rules you can start with.

The first breakdown we have is identifying if the problem is a contention or algorithm performance issue. This maps almost directly to our throughput vs. latency talks from the first section. These are the vast majority of performance issues you might encounter. Contention is what prevents us from getting the throughput that we want.

Algorithm performance affects latency. Everything works as expected, with little contention, but it's still not fast enough. These problems, while they're more fun to fix, can also mean huge changes. Making a sorting algorithm or a queue implementation or a face detection algorithm faster is an example of algorithmic performance.

> NOTE: While this article is mostly about identifying performance issues in your application, do not overlook the importance of identifying and fixing performance issues in software that your app depends on. Database servers are a common source of issues, for example. You should be running the contention checks on these services if your tests slowness in that area

## Algorithm Performance

Since we are going to spend the majority of our time talking about contention, let's spend a few minutes talking about algorithm performance. I consider an algorithm performance problem one where increasing the speed of the computer is the only way to improve performance. All contentions are minimized, but the code is running into fundamental limits from the hardware.

### Big-O

What we are talking about is the basis for Big-O notation. When you see O(n) or O(1) or O(n^2), these are Big-O examples. It helps us figure out the behavior of the algorithm with no restrictions. A merge sort (O(n log n)) is faster than a bubble sort (O(n*n)) because it needs to consider fewer things.

### Data Structures

This type of performance issue can also arise when you use improper data structures. This is why it's important for programmers to at least be aware of the differences between arrays, linked lists, trees, and maps.

### Caching

The last bit of note here is one we have all probably done: caching. If you've ever used ehcache or terracotta or memcached or even Hibernate, you've taken advantage of caching. The ability to skip a lengthy computation if the value doesn't change too frequently can mean the difference between 2 and 100 servers to handle the load you want. This can often be used for a quick performance gain if it's your first pass through the application.

## Contention

A computer has a finite amount of resources. Contention is when one of these resources is utilized to its maximum capacity, and there is a queue of work waiting to be done. That resource simply can't handle any more work. Common sources of contention are discussed below, as well as some ways to mitigate the effects.

### Disk Contention

Disk contention is when we spend too much time reading and writing data on disk. Because a spinning disk needs to get to the point on disk where you dsata lives, it takes some amount of time to get data back. If you have a lot of read and write tasks, your request may queue up behind others who haven't been served yet. This will result in long i/o wait times.

To investigate this issue on *nix-like systems, use the tool iostat. An example of a healthy disk setup:

Healthy Setup

You can see all the different devices in this example (thanks to Roger for putting it on the internet). The interesting things are the avgqu-sz and await. avgqu-sz is the average size of the request queue. In a mostly-idle system, this will be somewhere around zero, which means all requests are served quickly. In a system with disk contention, this number will grow rapidly. Await is the average time (in milliseconds) that a request took to process -- including queue time and serving time. Let's look at an unhealthy iostat:

Unhealthy iostat

There are a lot more devices on this machine, but you can see certain devices have a large queue size, and you can see the await times. If you want to serve a webpage in 100ms, but your disk is taking 105ms to service a request, you're going to have a bad time.

#### Blocking vs. Non-Blocking IO

You've surely heard about node.js by now. It's famous for its non-blocking IO. Whenever your application does something that needs to talk to disk (or something that would cause a wait), it yields to another task, and lets that original task finish later when the data is available.

Java also has some of this. There's a java.io package for blocking IO, and it's probably what most of us use. There's also the java.nio package for non-blocking IO. If you find that waiting for disk becomes an issue, see if you can use non-blocking IO calls to speed up your server's ability to handle more.

#### Reducing Contention

To reduce disk contention, there are four common strategies. First, you can buy your way out. Solid-state drives (SSD) are significantly faster than spinning disks for random seeks. If your access pattern is a lot of small reads and writes, this will get you very significant benefits in performance. If, instead, your performance problem is one of reading and writing very large chunks of data, an investment in an enterprise storage system may be warranted. A RAID setup combines multiple disks into one, providing (usually) both fault tolerance and increased throughput, since multiple drives contribute to the end result.

The second common strategy is to simply reduce the amount of data that is on the disk. If you can get by with smaller files, that may increase speeds. Compression can be used on large files, as expanding it in-memory can be faster than reading uncompressed from the disk. A different serialization format can result in significant disk savings (JSON, for example, is quite verbose compared to CSV).

The third common strategy is to do batch reads/writes. This is what your hard drive controller does, which is why it's so dangerous to just unplug your computer. There is a disk cache that waits for some amount of data, then writes it all to disk at once. This is how some high-performance NoSQL engines work. They keep as much working data in memory as possible, and flush to disk periodically.

The fourth common strategy is to do more caching. Your operating system likely does some of this for you, by caching frequently-used files in memory. Your application can do the same. If the data on disk changes infrequently, you might want to read it to memory when your app starts up. If it's very large, and you do a lot of searching, see if you can index the file, or sort the file, so you can use faster algorithms like a binary search to get to your data.

This problem can be magnified if you use disk stores backed by networks. NFS, NAS, Samba, SAN -- these are all disks backed by network. While they may offer unparalleled data security and storage capacity and data mobility, you incur some overhead since it needs to communicate over a network. That leads us to our next step...

### Network Contention

Networks have a lot of the same issues we have been discussing (latency, throughput, queueing). There's also the issue of network card capacity and network connection capacity. Most servers these days should have a gigabit ethernet card, and high-end servers should be using 10gb ethernet. But if your switches/routers are only capable of 10mbit, you will have a problem.

To look for potential network issues, there are two tools that are useful. First, there's the venerable netstat. This tool is useful to inspect network connections on the server. This can help you diagnose if you need additional threads to handle connections, if you're leaking connections, or if your system is being overwhelmed by connections. Second, there's a little utility called iftop. It periodically tracks the throughput of all network devices. It can drill down to a single connection, aggregate all connections, and track peaks and means. If you have a 100mbit network card and see ~10MB/sec in iftop, there's a good chance you're maxing out your network. Here's a little screenshot of it running with -t (to just print text to the console instead of an interactive app):

### iftop

Solving network issues can be tricky business. Lucky for us, it's also probably not the source of problems. I have seen it be the source of problems one or two times. In those cases, if you took a thread dump of the process under load, you would see multiple threads blocking on a process that is waiting in the function socketRead0 or socketWrite0 -- native code that actually does the socket communication. Sadly, thread dumps are the most reliable way I've found to differentiate between network issues and other forms of contention.

## CPU Contention

CPU contention is fun, in a way. We all know the command 'top' to display system load and running processes. In the modern world, with lots of threads and processes, it can be helpful to get a bit more detailed in our analysis. The tool htop breaks down each core's load, and provides a lot more than the default top:

htop

Notice how all 4 cores are showing a high level of usage in this screenshot. I highly recommend this tool to verify that your CPU is overloaded.

While this is one of the easiest metrics to inspect, it can be very difficult to fix. The system just has too much to compute to do much more. This is where algorithmic fixes and better data structures come into play. If you notice the system bogging down but the CPUs are not maxed, you have some other contention slowing it down.

### System Load

The load average from the above screen shot shows the 1-, 5-, and 15-minute averages of load. The load average is an exponentially dampened snapshot of running and runnable processes correlated with wait queue length. With multi-core machines, it gets even trickier. If it was truly just CPU, a 4-core machine with a load average of 1.00 is ambiguous. Is that 1 core fully maxed (and thus ripe for paralellization), or all 4 cores running at 25%? Maybe 2 cores at 50%? With multiple cores, a load average of 4 doesn't necessarily mean the system is under any stress. This is where tools like htop above help diagnose the issue

## Memory Contention

Memory contention is when there is more memory being used than available on your system. Thanks to swap partitions, this shouldn't crash a machine. However, it's likely to destroy your performance. The more tuned your app is, the more detrimental swapping will be. Memory contention can also come when you run into out-of-memory (OOM) problems, or issues with garbage collection in managed apps.

Swap-based issues are easy to spot, as top or htop will tell you the amount of swap they're using. For a production system, you ideally want no swap used. Putting 128GB or more on one machine is perfectly doable these days.

Out-of-memory usually surfaces as a process that dies unexpectedly with no messages. The only way to fix this is to consume fewer resources. This, again, is where better data structures or more compact object representations may help. You may also have memory leaks.

### Garbage Collection

Garbage collection tuning, especially in Java, is almost a job unto itself. There are a lot of tunable parameters. There is the permgen vs. the heap. There's the issue of heap resizing vs. fixed-size heap. When you profile your Java app, you really want to see the classic saw-tooth pattern:

GC-Sawtooth

This is a generally healthy system. The garbage collector kicks in and returns the heap to about the same size. A more unstable or growing-memory system looks like this:

GC-Bad

Notice how the "free heap" blue line climbs, then eventually drops, despite garbage collection happening on the green line. When the free heap is near zero, Java will spend a lot of its time trying to free up the heap, including multiple stop-the-world pauses. These can range from a second or so to multiple minutes, depending on heap size and object counts. If you let it run like this long enough, it will probably become unresponsive and eventually crash with an out-of-memory error.

Tuning the GC is beyond the scope of this article, but it can dramatically improve performance.

### Memory Leaks

The last thing to look out for is memory leaks. Technically it's impossible to do this in Java if you're not using unsafe libraries, but in practice it's a problem. The biggest issue is dangling references to objects that you no longer need. It keeps these objects around in perpetuity, and that could eventually kill your heap. Using tools described in the previous article (visualvm, jprofiler, yourkit), you can inspect where these objects are created, which ones take the most space, and what types of objects they are. This can be very helpful in tracking down excessive memory usage.

There is a fascinating issue in Java that has cropped up occasionally. If you have a very large string (say, a JSON or XML document, or a large text file you've put into a string), then take a substring of it, that substring is just a windowed view of the larger text string. This is good when your strings are small, as it prevents a lot of reallocation. However, when the source is very large, your substring holds a reference to that large document, meaning your memory usage is far larger than normal. This "leak" was fixed in OpenJDK 7u6. If you're still on JDK 5 or 6, you're probably being affected by this.

## Lock Contention

This is where things get really tricky. I've also found that it's the source of a lot of issues, so it's good to get well versed in this kind of contention. Because multiple threads accessing the same resource (variable, array, database connection, etc) can stomp on each other, there needs to be a way to control access to these sensitive bits. In Java, we often put synchronized on the method definition, or put a critical block inside a synchronized block. Behind the scenes, it creates a lock so that only one thread can execute here at a time.

When there are a lot of processes vying for the same lock, you are slowing them all down. If your locks are implemented as spin locks, it may exhibit 100% cpu usage while it waits. If you take a stack trace, you will see "Waiting on 0xXXXXXXX" for threads that are waiting for a lock. A useful tool for some users is TDA, and this is how it presents there:

### Thread Dump

Not all locks are deadlocks. Deadlocks are two threads waiting for locks that the other holds, with no way to make progress. Most profilers offer tools to automatically detect deadlocks, and will make your life significantly easier.

Lock contention is so expensive that multiple languages/frameworks/patterns are designed to avoid it. Functional programming languages often avoid this issue because they do little-to-no variable mutation. Immutable data structures simplify your life significantly. Because they can't change, it's safe for multiple threads to access the data. Lock-free queues are popular in some circles, but they can be nasty to code up correctly if you're not extremely well-versed in this specialty.

### Mechanical Empathy

If you're going the ultra-high-performance route, there's a concept you should be aware of called mechanical empathy. This describes a way of organizing your parallelism to minimize the burden on the CPU, especially context-swaps. When you're trying to design sub-millisecond responses in a managed language, it can be difficult to achieve using traditional methods.

Mechanical empathy was invented and popularized by the LMAX Disruptor. From the PDF of the whitepaper, this graphic comes to explain the cost of locks:

#### Lock Cost

You see that as contention (and thus arbitration) increases, your throughput gets tanked. LMAX Disruptor is an attempt to design a system to minimize locking and minimize expensive context switches.

This is not a simple implementation, and it's not a drop-in replacement for, say, ConcurrentHashMap. Read the link above to get a lot more information and see if it's the right approach for you.

# Conclusion

This wraps up our tour of performance analysis and tuning. Becoming proficient at analyzing and tuning the JVM as well as analyzing and tuning your own applications, you will have a much deeper understanding of your own code, as well as a much deeper understanding of how the JVM works, which might make for better code. Go forth and profile!
