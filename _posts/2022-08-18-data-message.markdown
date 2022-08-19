---
layout: post
title:  "How many bytes can you fit an object into?"
date:   2022-08-18 9:00:00 -0600
categories: Backend Messages Marshalling Data MessagePack
---

A running program handles data by keeping it in memory. Some languages may abstract this away from you, but at some point down the stack your program is
manipulating data stored in locations in memory. Sometimes, you might want to send this data to another function so it can perform some operation on it. Functions do this by taking the data [by reference](https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_reference). That is, they accept as an argument a memory location, and make its operations by _referencing_ that memory location.

{% highlight c %}
struct Point {
    int x; 
    int y;
}

void movePoint(Point* point, int x, int y) {
    // operates on a specific point by going to its location in memory
    point->x += x;
    point->y += y;
}
{% endhighlight %}

However, program memory is specific to the machine (and process!) the program is running on. Suppose that `movePoint()` was actually a function running on a different 
machine that we could call. I could pass in a memory location to a `Point` at some location like `0x1FFFFF21`, but on that other machine where `movePoint()` 
lives `0x1FFFFF21` could be the location of some completely different data. So, how can I represent the data running in memory on my computer so that other
machines (maybe over the network) could understand it? 

As a sidenote, we might still run into this problem with only one machine. If I were to write the memory location `0x1FFFFF21` to a file, and have my `movePoint()` function read from that file to get that memory location, that memory location might not make sense by the time `movePoint()` gets to it. This is especially the case if `movePoint()` is invoked by a different process than the one that wrote the memory location to disc. Memory is really only relevant to the exact same process you are running in, and persisting a memory location probably isn't a good strategy. 

# Serialization 

One thing we could do is take our data, like our `Point` struct, and write out its data in a binary format. We could pass `movePoint()` a long sequence of bits 
and as long as `movePoint()` knows how to decode that data it could do calculations with it. 

We could accomlish this with JSON. We take our data, and represent it using **J**ava**S**cript('s) **O**bject **N**otation, then just encode that representation
as a bunch of bytes. For example, a `Point` object could look like: 

{% highlight json %}
{
    "x": 2, 
    "y": 3
}
{% endhighlight %}

For the sake of demonstration, lets use a more complicated example. We could take a PHP class: 

{% highlight php %}
class User {
    public string $name; 
    public int $totalVictoryRoyales; 
    public array $nicknames;
}
{% endhighlight %}

and encode it in JSON as: 

{% highlight json %}
{
    "name": "Harris", 
    "totalVictoryRoyales": 0,  
    "nicknames": ["duckypotato", "ducky"]
}
{% endhighlight %}

Now that I have some JSON text that represents my data, however in text form we still need to turn it into binary so that languages other than Javascript 
can make heads or tails of it. We could just encode each of those ASCII bytes and send a big long message, but we can do slightly better. One algorithm that takes some JSON and gives us a binary encoding is [`MessagePack` ](https://msgpack.org/index.html). What we do in MessagePack is use byte control codes to tell the 
receiver of the message what kind of data will follow and how much of it there is. If we did this in spoken language, it would be like we said: "I'm about to tell you 5 words", then we said "Hello My Name Is Harris".

Our original 
message above occupies 77 bytes with whitespace removed. Let's walk through `MessagePack` and see how many bytes we can shed! Running our message through 
`MessagePack` gives us a 63 byte message: 

{% highlight hex%}
83 a4 6e 61 6d 65 a6 48 61 72 72 69 
73 b3 74 6f 74 61 6c 56 69 63 74 6f 
72 79 52 6f 79 61 6c 65 73 00 a9 6e 
69 63 6b 6e 61 6d 65 73 92 ab 64 75 
63 6b 79 70 6f 74 61 74 6f a5 64 75 
63 6b 79
{% endhighlight %}

The first byte `83` is a code that indicates that the following message is an Object with 3 fields, the first 4 bits give us `8` for object and the last 4 bits tell us the number of fields. 

Next, we have `a4`. The first four bits `a0` tell us that we have a string that is `4` bytes long (see the pattern yet?). This means the bytes `6e 61 6d 65`
represent `name` in ASCII, the name of our field! In the following byte we see `a6`, which is a 6 byte string, so `48 61 72 72 69 73` must stand for my name (Harris). 

Now, the next byte is `b3`. The first 4 bytes of this number is still `a0`, which means we are still looking at a string, and that string is `b3 - a0 = 13` bytes 
long (thats 19 in decimal). This is the bytes `74 6f 74 61 6c 56 69 63 74 6f 72 79 52 6f 79 61 6c 65 73` which is our field name `totalVictoryRoyals`. Next we have
`00`, `0` being a code for positive fixed integer and our value being `0`. Just like before, `a9` is telling us we have a 9 character string which is the 
name of our array, `nicknames`. 

The next byte is `92`. `90` tells us we have an array, and `02` tells us that array has two elements. Those two elements are encoded just like before, we have
two strings. 

That's a lot to follow, so lets break it down like this: 

|Code|Meaning|Value|
|----|-------|------|------|
|83  | Object w/ 3 fields | |
|a4  | 4 character string |`6e 61 6d 65`  |
|a6  | 6 character string |`48 61 72 72 69 73`  |
|b3  | 19 character string |`74 6f 74 61 6c 56 69 63 74 6f 72 79 52 6f 79 61 6c 65 73`  |
|00  | integer whos value are all the lower order bytes| 0  |
|a9  | 9 character string |`6e 69 63 6b 6e 61 6d 65 73`  |
|92  | 2 element array | `ab` 11 Char String, `a5` 5 Char String |

Not bad! We can shed a LOT more bytes with things like [ProtocolBuffer](https://en.wikipedia.org/wiki/Protocol_Buffers) which can pack our same
message into a lot fewer bytes. These protocols do this by not encoding the names of our fields. Instead, we specify ahead of time using a schema
definition what our objects are going to look like. For example, 

{% highlight php %}
message User {
    required string name = 1 
    required int64 totalVictoryRoyales = 2;
    repeated string nicknames = 3;
}
{% endhighlight %}

Now, when we go to encode `totalVictoryRoyals`, instead of having to encode all whopping 19 bytes of that field, we simply encode the `2` from our
schema. This is called a Field Tag. Then, the ProtocolBuffer library can look up what that 2 means, and decode the bytes properly. By avoiding the `32` bytes we needed JUST to encode field names, we can pack our message into a lot less bytes. We can also save our byte to indicate that we have encoded an object
because the ProtocolBuffer schema is already encoding that. 

It might look something like this

{% highlight hex%}
0a 06 48 61 72 72 69 73 10 00 1a B0 
64 75 63 6B 79 70 6F 74 61 74 6F 0A 
1a 05 64 75 63 6B 79 0A
{% endhighlight %}

which breaks down as

|Code|Meaning|Value|
|----|-------|------|------|
| `0a` | Field Tag 1, Type String |length: `06`: `48 61 72 72 69 73` (Harris)  |
| `10`  | Field Tag 2, Type Int | `00`  |
| `1a` | Field Tag 3 | length: `B0`: `64 75 63 6B 79 70 6F 74 61 74 6F 0A` (duckypotato) |
|`1a`  | Field Tag 3| length: `05`: `64 75 63 6B 79 0A` (ducky)  |

Note that we also don't need to encode any information about our last 2 fields being part of an array. Since we tagged them as being in field tag 3, 
ProtocolBuffer can figure out it needs to place them inside an array. So, our byte count is down to `32`. Not so bad.

# Why this is important
Okay, we just spent a LOT of time in the weeds of how we can encode information in binary. I'm a little worn out from writing that, and you're probably
a little confused. Let's take a massive step back and get some context for why we care. 

We want to be able to encode data in such a way that it can still be relevant to something that is not the same OS level process that created that data. 
This situation can come up as:
- Sending or requesting data from RESTful API's 
- Sending a message to a message broker such as [RabbitMQ](https://www.rabbitmq.com/) or [Kafka](https://kafka.apache.org/)
- Using [Remote Procedure Calls](https://en.wikipedia.org/wiki/Remote_procedure_call) such as [gRPC](https://grpc.io/)
- Passing messages between concurrent processes as actors in a actor system such as [Akka](https://akka.io/) 

We could do this decently enough with the built in serialization that some languages come with, such as Java's [built in serialize ](https://docs.oracle.com/javase/tutorial/jndi/objects/serial.html) that Akka uses. WE can use JSON encodings like MessagePack to compatability with most web services. We can also use ProtocolBuffer to drastically reduce the 
amount of space messages take up, at the cost of the up front setup needed to write custom schemas and run a compiler to turn those schemas into language
specific decoders. 

# References 

I made heavy use of the examples that Martin Kleppmann uses in Chapter 4 of [_Designing Data Intensive Applications_](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/), with some small modifications made so I had to work through the bytes myself. You should definitely 
read this book! 





