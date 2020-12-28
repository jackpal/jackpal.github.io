---
date: 2010-01-22 10:55
tags: go,bittorrent,Taipei-Torrent
title: 800 lines of code for a bencode serializer?!
---

> Wow, my Taipei-Torrent post made it to Hacker News and
reddit.com/r/programming. I'm thrilled!

One comment on Hacker News was "800 lines for a bencode serializer?! It only
took me 50 lines of Smalltalk!" That was a good comment! I replied:

> A go bencode serialization library could be smaller if it just serialized
the bencode data to and from a generic dictionary container. I think another
go BitTorrent client, gobit, takes this approach.
>
> But it's convenient to be able to serialize arbitrary application types.
That makes the bencode library larger because of the need to use go's
reflection APIs to analyze and access the application types. Go's reflection
APIs are pretty verbose, which is why the line count is so high.
>
> To make things even more complicated, the BitTorrent protocol has a funny
"compute-the-sha1-of-the-info-dictionary" requirement that forces BitTorrent
clients to parse that particular part of the BitTorrent protocol using a
generic parser.
>
> So in the end, the go bencode serializer supports both a generic dictionary
parser and an application type parser, which makes it even larger.

In general, Taipei-Torrent was not written to minimize lines-of-code. (If
anything, I was trying to maximize functionality per unit of coding time.) One
example of not minimizing lines-of-code is that I used a verbose error
handling idiom:

```
a, err := f()
if err != nil {
  return
}
```

There are alternative ways to handle errors in go. Some of the alternatives
take fewer lines of code in some situations. The above idiom is my preference
because it works correctly in all cases. But it has the drawback of adding 3
lines of code to every function call that might return an error.

The go authors are considering adding exceptions to the go language. If they
do so it will probably dramatically improve the line count of Taipei-Torrent.
