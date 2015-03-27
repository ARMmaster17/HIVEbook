# Introduction to the HIVE engine
This section is intended for system engineers who will be dealing with the hardware side of operating a hive. Software developers may skip this section if they wish. HIVE is developed so that programmers don't have to know how the hardware works, they just need to know that it works. The reverse is also true. However it may be useful to know how the underlying HIVE engine works when you run into issues like race conditions, which are common in multi-threaded or distributed computing.
## Foolish assumptions
By reading this part of the book, I assume that you have a strong background in the following:
* Enterprise networking
* System provisioning
* Hyper-V/virtualization (depends on your needs)
* RAM and SWAP space
* Latency and ping
* File permissions in NTFS

Unfortunately, unlike the programming side, there is not much help I can offer if you are lacking one of the above skill sets. When it comes to hardware setups, there is no replacement for real-world experience. A book can teach you about individual parts of a computer system, but only experience will teach you how to mix those concepts into working ideas. If you have ever dealt with a cluster running MPI or similar software, that qualifies too.

I also assume that you have already gone through Part 1 of this book. That includes following along with the examples on your own hardware. If you don't already have a functional HIVE setup, I suggest you take care of that now. Learning isn't a spectator sport.