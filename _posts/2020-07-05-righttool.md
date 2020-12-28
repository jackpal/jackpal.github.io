---
date: 2020-07-05 15:48
description: Picking the right tool for the job. Helping my daughter write a model rocketry data collection app.
tags: Google_Sheets Swift_UI Web
title: Picking the Right Tool for the Job
---

Over the past few months I have been coaching one of my daughters as she writes a data collection application for her model rocketry team.

Her team is competing in a yearly model rocketry contest called [The American Rocketry Challenge](https://rocketcontest.org) (TARC). Teams of high school students compete to design model rockets that best meet contest rules. Like a road rally race, the goal is not to build the fastest or highest flying rocket, but rather to build a rocket that can most precisely fly to a given height, with a given flight duration.

In the course of a year, a team typically makes around 30 test flights, carefully modifying their rocket to more closely meet the contest criteria.

My daughter wanted to create an application to enable her team to record the flight data (altitude, flight time, weather, whether the payload egg cracked, etc.) for all their flights. Once collected, she wanted to be able to analyze the data. (Graphing it, computing averages and deviations, and so forth.)

She initially wanted to write a mobile phone app to do this, so I helped her investigate the how to do this. We settled on [Google Firebase](https://firebase.google.com).  I helped her write a prototype iOS app using [SwiftUI](https://developer.apple.com/xcode/swiftui/). It worked well, and looked great, but it turned out to have a few drawbacks:

+ Firebase servers are complicated to set up, and can’t easily be cloned. This steered us towards a design where we had one application for multiple contest teams. Once we started down that design path, we ended up with a hierarchical design with “organizations”, that had “teams”, that had “members”. There was an account system with user roles such as “organization administer”, “team administrator”, “team member”, and so on. Complicated server-side rules and scripts enforced different permissions.

+ Apple’s “Sign in With Apple” product is attractive for users, but is difficult for app developers. Users typically don’t know their Apple account email, which makes it difficult to help them administer their accounts. Working around this required us to implement a complicated invitation system.

+ The app itself was about 1500 lines of Swift code, and we were not enthusiastic about porting it to Android.

We were not sure that we were going to be able to get both the iOS and Android versions of the app finished in time for the fall launch season. Plus we weren’t sure we wanted to deploy an app that required centralized administration.

So in late June we had a re-think.  We came up with a simpler solution: Write the app as a [Google Sheets](https://www.google.com/sheets/about/) spreadsheet. This is a clunkier UI, but it has a number of important benefits:

+ Works on Android, iPhone, and PC.
+ Each team has its own independent spreadsheet.
+ No central administration.
+ Uses the normal Google Docs account system.
+ Avoids the potential of hitting the “free tier” Firebase account limits.
+ Each team can customize the sheet to taste, using easy-to-understand spreadsheet formula and scripts.
+ Powerful charting and data analysis tools are built in.
+ Allows quick-and-dirty end-user changes to UI.
+ Concentrates on solving the “Data” part of the problem, rather than the “Design” part.

Since she switched technologies, development has gone much more quickly. Partly because the spreadsheet provides so much built-in structure, and partly because it de-scoped all the multi-team and account role related work.

The main drawback of the new tech stack is that the Google Sheets mobile app UI is limited. The new app definitely looks like a spreadsheet app rather than a mobile app. For example, instead of pressing a button to invoke a script, users have to pick a menu item from a cell’s pop-up data validation menu.

But it’s such a relief to be essentially “done” with development of the first version of the app. Now she’s working on creating a web site, writing documentation, recording tutorial videos, running user tests, and all the other work that’s needed to polish and launch the app.

I guess her greatest challenge is waiting to see if next year’s TARC contest happens at all, given the COVID-19 pandemic.

The app’s web site is [Yes it’s Rocket Science](https://yesitsrocketscience.github.io)