---
date: 2022-04-29
description: A level solver for Hitman Go style puzzle games.
tags: Swift SwiftUI Puzzle Hitman_Go iOS A-Star AI
title: A Solver for Hitman Go Levels
---

I wrote a solver for [Hitman Go](https://en.wikipedia.org/wiki/Hitman_Go)
levels. You can use it to:

+ Solve existing levels.
+ Design and test your own levels.
+ Study graph search algorithms like A-Star.

The code is available in four related projects:

+ [SpyPuzzleGameState](https://github.com/jackpal/SpyPuzzleGameState)
The data structures for game levels.
+ [SpyPuzzleSolver](https://github.com/jackpal/SpyPuzzleSolver) An A-Star-based level solver.
+ [SpyPuzzleCLI](https://github.com/jackpal/SpyPuzzleCLI) A command-line tool for solving levels.
+ [SpyPuzzleApp](https://github.com/jackpal/SpyPuzzleApp) An iOS app for interactively testing and solving levels.

![Screenshot of Spy Puzzle App](/assets/posts/2022-04-29-SpyPuzzleApp.png)

In this screenshot of a simple test level, the "Agent" needs to pick up
a red key to open the red door, followed by the blue key to open the blue
door. The solver has correctly solved the moves required to solve the
level as "east, east, south".

<!--more-->

This project started over the 2021 holiday break. I was playing
[Hitman Go](https://en.wikipedia.org/wiki/Hitman_Go), and I was stuck on
a difficult level. I decided to write a little Python script to solve the
level. It only took a few hours to write the script.

That naturally lead to extending the script to solve more levels, then
switching to Swift for type safety, then writing a UI to make it
easier to debug, and here we are today.

The code can represent any Hitman Go level, and knows the rules for the
game. All node types, edge types, enemy types, and items are represented.

In theory the solver should be able to
solve any solvable game level. It can solve some levels very quickly,
while other levels are effectively unsolvable due to taking too much time or
memory. I am using the A-Star algorithm and some simple heuristics.

The current v 0.1.0 solver can quickly solve over half of all Hitman Go levels:

Status     | Count
------     | ----:
Fast       |  66
Too slow   |  25
Not tested |  21
Total      | 121

The "not tested" levels are levels that seem more complicated than the
"too slow" levels, so I haven't added them to the test suite yet. I will
add them once the "too slow" levels have been solved.

The typical reason that a level can't be solved in reasonable time is that
there is an important item (like a costume) locked behind a door. The
level solver doesn't understand that, so it take a long time exploring
dead ends before it tries to open the door.

# Benefits and drawbacks of using Swift

I started the project in Python, but switched to Swift because the Python
code quickly became difficult to work with.

Swift was a good choice:

+ strong type checking
+ enum types
+ value semantics for structs, Arrays, Sets, and Dictionaries
+ code-generating conformances for Hashable
+ A nice third party [A-Star](https://github.com/Dev1an/A-Star) implementation.
+ Swift UI made it easy to write an app for testing.
+ Swift Package Manager and Xcode made development and regression testing easy.

However there were some drawbacks to using Swift:

- Debug builds were slow. I developed using Release mode except when debugging.
- The value semantics seemed to be slow and use a lot of memory.

I think that a C++ implementation would probably
use less memory and run faster.

# Future work

If I had time, I would like to improve the solver to try solving more
levels. Some sort of "sub goal" strategy seems like it would be useful.
