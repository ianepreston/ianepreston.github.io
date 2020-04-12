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
