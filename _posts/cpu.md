# How do CPU speeds work these days?


1. TOC
{:toc}

## Introduction

I used to know how to spec out a computer. In the early 2000s I worked as a repair tech / sales guy at a local computer shop. Back then it was pretty easy. In general the CPU with more MHz was faster. End of story. These days it's more complicated. There's cores, and threads, and I don't really have a good handle on how to evaluate processors. To rectify this I'm going to write this post, where I compare a bunch of processors I own or have owned, plus maybe some that I don't, and figure out how they compare performance wise.

## The candidates

So these are the processors up for grabs:

- 10th generation Core i5-1035G1 3.6GHz w/6MB cache - processor in my new laptop
- Ryzen 7 1700X 3.4GHz w/16MB cache - my current desktop CPU
- Pentium G3258, 3.2GHz w/3MB cache - my old desktop CPU
- 7th generation Core i5-7200U 3.1GHz w/3MB cache - processor on my 3 year old cheaper laptop
- Athlon II X2 245 2.9GHz w/2MB cache - A CPU I bought ten years ago
- Core i3-4005U 1.7GHz w/3MB cache - processor on the minicomputer I'm going to load pfsense on.
- ARM Cortex-A72 1.5GHz w/1MB cache - Processor on my raspberry pi 4
- ARM Cortex-A53 1.2GHz - Processor on my raspberry pi 3
- Atheros QCA9558 0.72GHz - processor on my router

So, first couple notes. This is a wide array of processors. Everything from a weak commercial router CPU, to a ten year old desktop CPU, to a pretty decent and new desktop CPU. I'm hoping having this much variety will help explain how to understand how to evaluate this. The second note is that clearly clock speed alone is not enough. The Athlon II is ten years old and was cheap when I bought it. If I just look at clock speed my Ryzen is only 17% faster, but there's no way that's true. So what else do we need to know?

## What else to look into

After a bit of reading I think I've identified the other factors that are important. Let's take a look at them.


### Clock speed

This is the speed in GHz and specifies the number of cycles per second. Back in the day this was all that really mattered, and it's still important, but just less so now.

### Instructions per clock

Basically how efficient the CPU is. For every cycle (which it will do more of per second as clock speed increases) how many instructions does the CPU complete? I don't see this stat described very often, but it seems like it would be handy to know.


### Cores

This is the pretty easy to understand. Back when I was first learning about computers, processors only had a single core. Now almost all CPUs have multiple cores. Multiple cores are essentially the equivalent of multiple CPUs on one chip. While they won't matter for performance on individual tasks that aren't optimized for them, they are effectively a multiplier in terms of performance when it comes to multitasking, or any applications which can use multiple cores.

### Threads

Threads are conceptually similar to cores, in that they are a kind of multiplier. However, while cores are actual physical components of the CPU and directly impact the workload it can manage, threads are a measure of the number of tasks that can be passed to the cores simultaneously. I get the impression that threads are important for multitasking, and have some performance benefits in general, but are less critical to evaluating performance for most workloads than cores.

### Cache

The cache is a small but very faster (much faster than RAM) amount of memory located directly on the CPU. The larger the cache, the more efficiently work can be passed to the CPU. This is definitely important, but it seems like at a high level CPUs will have an amount of cache sufficient to handle their processing capabilitiest. So a CPU with a larger cache probably means it's better, but it's a fairly crude measure.

### Other considerations

Some CPUs draw more power and generate more heat than others, that's certainly something to keep in mind even if it doesn't directly impact performance. Second, lots of CPUs these days have integrated graphics processors. Assuming you have an external GPU I don't think that will really matter. It seems like all Intel CPUs these days have some onboard graphics, while only AMD APUs do.


## Evaluation

Having added in some more details around what matters for CPUs these days, I'll build a table with all of those stats (or as many as I can find) along with benchmarks for the processors.

