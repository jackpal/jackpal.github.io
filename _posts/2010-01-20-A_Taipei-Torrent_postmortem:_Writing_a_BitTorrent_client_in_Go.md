---
date: 2010-01-20 07:34
tags: go bittorrent taipei-torrent
title: "A Taipei-Torrent postmortem: Writing a BitTorrent client in Go"
---

_This is a BitTorrent client. There are many like it, but this one is mine._

 _\-- the BitTorrent Implementer's Creed_

For fun I've started writing a command-line BitTorrent client in Google's go
programming language. The program, [Taipei-Torrent](http://github.com/jackpal/Taipei-Torrent) ,
is about 70% done. It can successfully download a torrent,
but there are still lots of edge cases to implement.

Go routines and channels are a good base for writing multithreaded network
code. My design uses goroutines as follows:

* a single main goroutine contains most of the BitTorrent client logic.
* each BitTorrent peer is serviced by two goroutines: one to read data from the peer, the other to write data to the peer.
* a goroutine is used to communicate with the tracker
* some "time.Tick" goroutines are used to wake the main goroutine up periodically to perform housekeeping duties.

All the complicated data structures are owned by the main goroutine. The other
goroutines just perform potentially blocking network I/O using channels,
network connections, and []byte slices. For example, here's a slightly
simplified version of the goroutine that handles writing messages to a peer:

```
func (p *peerState) peerWriter(errorChan chan peerMessage,
  header []byte) {
  _, err := p.conn.Write(header)
  if err != nil {
    goto exit
  }
  for msg := range p.writeChan {
    err = writeNBOUint32(p.conn, uint32(len(msg)))
    if err != nil {
      goto exit
    }
    _, err = p.conn.Write(msg)
    if err != nil {
      goto exit
    }
  }
exit:
  errorChan <- peerMessage{p, nil}
}
```

Good things about the Go language and standard packages for this project:

* Even without IDE support it is easy to refactor go code. This ease-of-refactoring makes it pleasant to develop a program incrementally.
* Goroutines and channels make networking code easy to write.
* The log.Stderr() function makes debugging-by-printf fairly painless.
* Go maps serve as pretty good one-size-fits-all collection classes.
* I received very fast responses from the go authors to my bug reports.
* The standard go packages are reliable and easy to use. I used the xml, io, http, and net packages pretty extensively in this project. I also used the source of the json package as a base for the bencode package.
* gofmt -w is a great way to keep my code formatted.
* Named return values, that are initialized to zero, are very pleasant to use.
* The code writing process was very smooth and stress free. I rarely had to stop and think about how to achieve what I wanted to do next. And I could often add the next feature with relatively little typing. The feeling was similar to how it feels when I write Python code, only with fewer type errors. :-)
* With the exception of using the reflect package I never felt like I was fighting the language or the compiler.

Minor Problems:

* It was a little tedious to write if err != nil {return} after every function call in order to handle errors.
* The standard go http package is immature. It is missing some features required for real-world scenarios, especially in the client-side classes. In my case I needed to expose an internal func and modify the way a second internal func worked in order to implement a client for the UPnP protocol. The good news is that the http package is open source, and it was possible to copy and fork the http package to create my own version.

What wasted my time:

* Several hours wasted debugging why deserializing into a copy of a variable (rather than a pointer to the original variable) had no effect on the value of the original variable. I notice that I often make mistakes like this in go, because go hides the difference between a pointer and a value more than C does. And there're confusing differences in behavior between a struct (which you can pass by either value or reference) and an interface (which has reference semantics on its contents even though you pass the interface by value). When you are reading and reasoning about go code you must mentally keep track of whether a given type is a struct type or an interface type in order to know how it behaves.
* Several hours wasted figuring out how to send and receive multicast UDP messages. This may be an OSX-specific bug, and it may already be fixed. I found the Wireshark packet sniffer very helpful in debugging this problem.
* Several hours wasted with crashes and hangs related to running out of OS threads on OSX. This was due to my code instantiating time.Tick objects too frequently. (Each live time.Tick object consumes a hardware thread.)
* Many hours spent trying to understand and use the reflect package. It is powerful, but subtle and mostly undocumented. Even now, some things remain a mystery to me, such as how to convert a []interface{} to an interface{} using reflection.

Project statistics:

* Line count: Main program: 1500 lines, http package patches: 50 lines, UPnP support: 300 lines, bencode serialization package: 800 lines. Tests for bencode serialization: 300 lines
* Executable size: 1.5 MB. Why so large? One reason is that go doesn't use shared libraries, so everything is linked into one executable. Even so, 1.5 MB seems pretty large. Maybe go's linker doesn't strip unused code.
* Development time: ~8 days so far, probably 11 days total.
* The source is available under a BSD-style license at: [github.com/jackpal/Taipei-Torrent](https://github.com/jackpal/Taipei-Torrent)

Edits since first post:

* Added error reporting to code example.
* Added a link to the source code.
* Give more details about the line count. I had mistakenly included some test code in the lines-of-code for the main program.
* Add a statistic about program size.
