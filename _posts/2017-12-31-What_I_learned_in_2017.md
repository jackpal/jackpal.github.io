
---
date: 2017-12-31 00:08
tags: Year in Review
title: What I learned in 2017
---

## Shipping an Audio Pipeline

In 2017 I shipped a new audio rendering pipeline for the iOS version of
[Google Play Music](https://itunes.apple.com/us/app/google-play-music/id691797987?mt=8).
I use it to render a particular flavor of fragmented
MP4 that we use in the Google Play Music streaming music service. It was quite
a learning experience to write and ship real-time audio code on iOS.

If you are looking to write an audio pipeline for iOS, I highly recommend
basing it on [The Amazing Audio Engine 2](https://github.com/TheAmazingAudioEngine/TheAmazingAudioEngine2).
Core Audio is a powerful library with an peculiar API. TAAE2 provides a much nicer
API on top of Core Audio, without adding much overhead.

I had designed and implemented much of my new audio pipeline in 2016, but 2017
was the year that I deployed the pipeline to production.

I learned that shipping an audio rendering pipeline comes with a long tail of
bugs, most of which have the same symptom: "I was listening to a song when it
stopped". I was able to find and fix most of my bugs by using a combination
of:

* Great base libraries. (TAAE2 and Core Audio)
* A clean, well-thought-out design.
* Unit tests.
* Assertions.
* Playback error logging that is aggregated on the server side.
* A/B testing. (Between different versions of my audio renderer code.)
* Excellent bug reports from alpha users.
  * My boss and my boss's boss's boss were the two most prolific bug reporters.
  * Several other alpha users went out of their way to help me diagnose bugs that affected them.

The error logging and A/B testing together gave me the confidence to roll out
the feature, by showing how well it performed compared to the previous
renderer stack.

## Learning a new code base

In 2017 I joined the YouTube Music team to work on  the iOS version of
[YouTube Music](https://itunes.apple.com/us/app/youtube-music/id1017492454?mt=8),
which meant that I had to get up to speed on the
YouTube Music code base. It's a large complicated app. I found it difficult to
get traction.

What finally worked for me was giving up my quest for general understanding. I
just rolled up my sleeves and got to work fixing small bugs and adding small
features. This allowed me to concentrate on small portions of the program at a
time, and also provided a welcome sense of progress. My understanding of the
overall architecture has grown over time.

## Learning Swift and UIKit

In 2017 I audited both the [iOS 10](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120) and
[iOS 11](https://itunes.apple.com/us/course/developing-ios-11-apps-with-swift/id1309275316) versions of the Stanford iOS programming class. In past
years I've just watched the lectures. This year I actually did the programming
assignments. These classes gave me a thorough understanding of Swift and
UIKit. I felt they were well worth my time.

The reason I audited both the iOS 10 and iOS 11 versions of the course is that
changes to the Swift language meant that the final programming assignment of
the iOS 10 version of the class can't be completed using Xcode 9. I was only
able to finish the first half of the iOS 10 version of the course. When the
iOS 11 version came out in December, I was able to resume my studies. I've
done the first 3 problem sets, and hope to complete more before the end of my
Christmas / New Years' Day holiday.

## Learning Machine Learning

I am fascinated by the recent advances in machine learning, especially the
[DeepMind](https://deepmind.com/) AlphaGo and Alpha Zero programs.

In 2016 I built a modest home PC capable of doing machine learning, but it sat
idle for most of 2017. I haven't been able to do much with machine learning
other than read papers and run toy applications. I am contributing computer
cycles to the [Leela Zero](https://github.com/gcp/leela-zero) crowd-sourced Go
player inspired by AlphaGo.

As a long-time client-side developer, it's frustrating that there's no "UI" to
machine learning, and the feedback loop is so long. I'm used to waiting a few
minutes at most to see the results of a code change. With machine learning it
can take hours or days.

It is also frustrating that there are so many different toolkits and
approaches to machine learning. Even if I concentrate on libraries that are
built on top of Google's TensorFlow toolkit, there are so many different APIs
and libraries to consider.

## Learning to let go of personal computing

In 2017 I continued to adapt to the decline of the personal computer. I am
gradually retiring my local, personal computer based infrastructure, and
adopting a cloud-based mobile phone infrastructure.

I'm surprised how smoothly the transition has gone. I still have laptops and
PCs around my house, but the center-of-gravity for computer use in my family
is continuing to shift to phones.

I'm happy that I'm spending less time on computer maintenance.

## Looking forward to 2018

My engineering learning goals for 2018 are:

* Finish the Stanford iOS 11 programming course.
* Write a small machine learning app.
* Help my kids improve their programming skills.

Hat tip to [Patrick McKenzie](https://twitter.com/patio11/status/946910434887471104) for the
writing prompt.
