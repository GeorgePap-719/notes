# Journal

## 21 - 07 - 2025

Reading through the technical paper of [LMAX Disruptor](https://lmax-exchange.github.io/disruptor/disruptor.html), I decided to
create a simple implementation of it, just to learn the nitty details. Also, it is more fun to create than to read.

After much research, it seems the [agrona-project](https://github.com/aeron-io/agrona) is the successor (kinda?), of this effort,
where it included more improvements on top of the technical-paper. Therefore, my implementation will resemble more the agrona-project. Martin Thompson (one of the creators of lmax-Disruptor and
agrona-project) has a nice [video](https://www.youtube.com/watch?v=929OrIvbW18&t=1658s) about the improvements
on top of the first Disruptor's version.

Here is the [implementation](https://github.com/GeorgePap-719/RingBuffer) for anyone that wants to take a look.

Anyway, my next though was to take this data-structure and make it compatible with the Kotlin's Channel implementation.
In practice this would mean, that whenever the Disruptor algorithm would spin or would block the thread, it would instead
"suspend" the current coroutine. This effort lead me to this the paper ["Fast and Scalable Channels in Kotlin Coroutines"](https://arxiv.org/abs/2211.04986). The paper itself is easy to read, and to
make sense of it.

My first attempt was to try to merge these two data-structures, but I never got to finish it. In theory, it seemed easy,
but it turns out the structures use fundamental different approaches.

Stuff to sort out:

- Disruptor uses a fixed-size array to keep the elements, but the BufferedChannel uses an "infinite-array".
  The channel follows this approach because it needs to keep track of all the "waiters" (suspending coroutines).
  If I want to merge these structures I cannot avoid this type of problem,
  but solving it forces me to alter the Disruptor pattern too much because at its "core" it operates with fixed-size cells.
- Disruptor is a FIFO structure, but the Channel marks this behavior as "incorrect" due to Channel's semantics.

For now, I will stop this effort and maybe come back at it later. 