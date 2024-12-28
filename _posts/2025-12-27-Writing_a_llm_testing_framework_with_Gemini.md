---
date: 2024-12-27 12:25:00
description: Report on writing a small Python app using Gemini 1206
tags: advent_of_code, gemini, python, vs_code, jupyter_notebook, flask
title: Using Gemini to write a LLM tester in Python
---

I recently used Gemini to help me participate in the [2024 Advent of Code](https://adventofcode.com/2024)
contest. Gemini 1206 was able to [solve 60% of the 2024 puzzles](https://jackpal.github.io/2024/12/24/Advent_of_Code_2024.html)
without any human assistance beyond the well designed puzzle specifications and
a carefully crafted prompt.

Two questions naturally arise:

1. Gemini comes in several flavors. How do they compare in their ability to
   solve Advent of Code puzzles?
2. How well will Gemini do on older years of Advent of Code?

I wrote a suite of Python programs to help me answer those questions.

As you probably guessed, I used Gemini to help me wrote these programs.

I wrote a detailed spec for a program that would:

   + Maintain a database of which puzzles had been solved by which model.
   + Pick which puzzle to solve next, using which model.
   + Use the model to generate a program to solve the puzzle.
   + Run the program.
   + Compare the program's output to the correct answer, and update the puzzle
         database.
   + Run a web server to show the results as a web page.
   + Have the ability to generate reports as markdown or csv.
   + Have the ability to perform simple database edits. For example, to remove
         puzzle test results when they need to be regenerated.

I constrained the design by requiring that:

  + The programs must be written in Python.
  + The test results must be stored in a SQLite3 database
  + The web server must use the Flask framework.
  + I provided a library with API functions to:
    + scrape puzzle descriptions
    + asking Gemini models to generate programs using the puzzle descriptions
    + run the generated programs to produce an answer
    + ask the AdventOfCode website to verify the answers.
+ I used the excellent [Advent of Code Data](https://pypi.org/project/advent-of-code-data/)
      package to scrape the puzzle descriptions and verify the answers. AOCD
      implements local caching of descriptions and answers, so I didn't have
      to implement that feature myself.

The process of implementing the program proceeded iteratively:

1. Write a detailed spec.
2. Paste the spec into [AI Studio](aistudio.google.com).
3. Paste the code into a Visual Studio Code project.
4. Debug the code under the Visual Studio Code Python Debugger.
5. Report errors and incorrect behavior to Gemini.
6. Update code as suggested by Gemini.
7. Sometimes I'd decide that I wanted to take another approach. To do that, I
   would update my detailed spec and go back to step 2. Because the spec
   specified the library API, I was able to reuse the library code with
   different versions of the generated code.

The biggest stumbling block for development was the limited AI Studio free
quota for API access. 1500 API requests per day doesn't go as far as I might
wish.

It took several rounds of experimentation and spec rewriting to settle on a
good design. I initially specified a single program to cover all the features.
But due to limitations of Unix forking, it turned out to work better to have
four separate programs, one for each feature, all communicating through a common
SQLite database.

Gemini's generated code was good. There were many minor mistakes, but most of
them were easy to fix by pasting the error message and stack crawl into Gemini.
Gemini would explain what it thought the error was, and would almost always
produce a working bug fix.

When Gemini ran into a dead end, and progress stalled, I would move forward by
rethinking my design. Then I would update my design and regenerate the code.

The next time I do a project like this, I think I will try asking Gemini for
help with my design document.

The puzzle runner source code is here: [github.com/jackpal/aocllmtest](https://github.com/jackpal/aocllmtest)

It will probably take another few days to complete the project, but I think
from now on it's mostly a matter of waiting for quota to regenerate, followed
by some statistic analysis of the collected data.

Some interesting potential follow-up projects:

+ Feeding puzzle solution code errors back to the models, to see if that
  improves their performance. It seems likely that this would help, since
  Gemini sometimes makes simple mistakes that it can easily fix if shown
  the resulting error message.
+ Trying other models like Claude and OpenAI. (Limited by limited free quota.)
+ Trying open source models like Llama. Limited by my slow personal hardware.
+ Using languages other than Python for the test harness code.
+ Using languages other than Python for the puzzle code.
