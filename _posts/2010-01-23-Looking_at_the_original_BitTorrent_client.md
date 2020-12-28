---
date: 2010-01-23 13:27
tags: go,BitTorrent,python
---

# Looking at the original BitTorrent client

I wanted to learn more about how the original BitTorrent client implemented
"choking", so I downloaded the source to the original Python-based BitTorrent
client from [SourceForge](http://sourceforge.net/projects/bittorrent/).

I was impressed, as always, by the brevity and clarity of Python. The original
BitTorrent client sources are very easy to read. (Especially now that I
understand the problem domain pretty well.) The code has a nice OO design,
with a clear separation of responsibilities.

I was also impressed by the extensive use of unit tests. About half the code
in the BitTorrent source is unit tests of the different classes, to make sure
that the program behaves correctly. This is a really good idea when developing
a peer-to-peer application. The synthetic tests let you test things like your
end-of-torrent logic immediately, rather than by having to find an active
torrent swarm and wait for a real-world torrent to complete.

When I get a chance I'm going to refactor the Taipei-Torrent code into smaller
classes and add unit tests like the original BitTorrent client implementation
has.

And come to think of it, why didn't I check out the original BitTorrent client
code sooner? It would have saved me some time and I'd probably have a better
result. D'Oh! Oh well, live and learn!
