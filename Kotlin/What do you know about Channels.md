# Channels
Deferred values provide a convenient way to transfer a single value between coroutines. Channels provide a way to transfer a stream of values. *Channel* is a non-blocking primitive for communication between a sender (via `SendChannel`) and a receiver (via `ReceiveChannel`).

## [Channel basics](https://kotlinlang.org/docs/reference/coroutines/channels.html#channel-basics)
A [Channel](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-channel/index.html) is conceptually very similar to `BlockingQueue`. One key difference is that instead of a blocking `put` operation it has a suspending [send](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-send-channel/send.html), and instead of a blocking `take` operation it has a suspending [receive](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-receive-channel/receive.html).

```
val channel = Channel<Int>()
launch {
    // this might be heavy CPU-consuming computation or async logic, we'll just send five squares
    for (x in 1..5) channel.send(x * x)
}
// here we print five received integers:
repeat(5) { println(channel.receive()) }
println("Done!")
```

The output of this code is:
```
1
4
9
16
25
Done!
```

## [Closing and iteration over channels](https://kotlinlang.org/docs/reference/coroutines/channels.html#closing-and-iteration-over-channels)
Unlike a queue, a channel can be closed to indicate that no more elements are coming. On the receiver side it is convenient to use a regular `for` loop to receive elements from the channel.

Conceptually, a [close](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-send-channel/close.html) is like sending a special close token to the channel. The iteration stops as soon as this close token is received, so there is a guarantee that all previously sent elements before the close are received:

```
val channel = Channel<Int>()
launch {
    for (x in 1..5) channel.send(x * x)
    channel.close() // we're done sending
}
// here we print received values using `for` loop (until the channel is closed)
for (y in channel) println(y)
println("Done!")
```

## [Building channel producers](https://kotlinlang.org/docs/reference/coroutines/channels.html#building-channel-producers)
The pattern where a coroutine is producing a sequence of elements is quite common. This is a part of *producer-consumer* pattern that is often found in concurrent code. You could abstract such a producer into a function that takes channel as its parameter, but this goes contrary to common sense that results must be returned from functions.

There is a convenient coroutine builder named [produce](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/produce.html) that makes it easy to do it right on producer side, and an extension function [consumeEach](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/consume-each.html), that replaces a `for` loop on the consumer side:

```
val squares = produceSquares()
squares.consumeEach { println(it) }
println("Done!")
```

## [Pipelines](https://kotlinlang.org/docs/reference/coroutines/channels.html#pipelines)
A pipeline is a pattern where one coroutine is producing, possibly infinite, stream of values:
```
fun CoroutineScope.produceNumbers() = produce<Int> {
    var x = 1
    while (true) send(x++) // infinite stream of integers starting from 1
}
```

And another coroutine or coroutines are consuming that stream, doing some processing, and producing some other results. In the example below, the numbers are just squared:
```
fun CoroutineScope.square(numbers: ReceiveChannel<Int>): ReceiveChannel<Int> = produce {
    for (x in numbers) send(x * x)
}
```

The main code starts and connects the whole pipeline:
```
val numbers = produceNumbers() // produces integers from 1 and on
val squares = square(numbers) // squares integers
repeat(5) {
    println(squares.receive()) // print first five
}
println("Done!") // we are done
coroutineContext.cancelChildren() // cancel children coroutines
```

## [Buffered channels](https://kotlinlang.org/docs/reference/coroutines/channels.html#buffered-channels)
The channels shown so far had no buffer. Unbuffered channels transfer elements when sender and receiver meet each other (aka rendezvous). If send is invoked first, then it is suspended until receive is invoked, if receive is invoked first, it is suspended until send is invoked.

Both [Channel()](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-channel.html) factory function and [produce](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/produce.html) builder take an optional `capacity` parameter to specify *buffer size*. Buffer allows senders to send multiple elements before suspending, similar to the `BlockingQueue` with a specified capacity, which blocks when buffer is full.

Take a look at the behavior of the following code:

```
val channel = Channel<Int>(4) // create buffered channel
val sender = launch { // launch sender coroutine
    repeat(10) {
        println("Sending $it") // print before sending each element
        channel.send(it) // will suspend when buffer is full
    }
}
// don't receive anything... just wait....
delay(1000)
sender.cancel() // cancel sender coroutine
```

It prints "sending" *five* times using a buffered channel with capacity of *four*:
```
Sending 0
Sending 1
Sending 2
Sending 3
Sending 4
```

The first four elements are added to the buffer and the sender suspends when trying to send the fifth one.

## Creating channels
The `Channel(capacity)` factory function is used to create channels of different kinds depending on the value of the `capacity` integer:

- When `capacity` is 0 — it creates a *rendezvous* channel. This channel does not have any buffer at all. An element is transferred from the sender to the receiver only when `send` and `receive` invocations meet in time (rendezvous), so `send` suspends until another coroutine invokes `receive`, and `receive` suspends until another coroutine invokes `send`.

- When `capacity` is `Channel.UNLIMITED` — it creates a channel with effectively unlimited buffer. This channel has a linked-list buffer of unlimited capacity (limited only by available memory). `Sending` to this channel never suspends, and `offer` always returns `true`.

- When `capacity` is `Channel.CONFLATED` — it creates a *conflated* channel This channel buffers at most one element and conflates all subsequent `send` and `offer` invocations, so that the receiver always gets the last element sent. Back-to-send sent elements are conflated — only the last sent element is received, while previously sent elements **are lost**. `Sending` to this channel never suspends, and `offer` always returns `true`.

- When `capacity` is positive but less than `UNLIMITED` — it creates an array-based channel with the specified capacity. This channel has an array buffer of a fixed `capacity`. Sending suspends only when the buffer is full, and `receiving` suspends only when the buffer is empty.

Buffered channels can be configured with an additional `onBufferOverflow` parameter. It controls the behaviour of the channel’s `send` function on buffer overflow:
- `SUSPEND` — the default, suspend `send` on buffer overflow until there is free space in the buffer.
- `DROP_OLDEST` — do not suspend the `send`, add the latest value to the buffer, drop the oldest one from the buffer. A channel with `capacity = 1` and `onBufferOverflow = DROP_OLDEST` is a *conflated* channel.
- `DROP_LATEST` — do not suspend the `send`, drop the value that is being sent, keep the buffer contents intact.

# Links
https://kotlinlang.org/docs/reference/coroutines/channels.html  
https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.channels/-channel/index.html
