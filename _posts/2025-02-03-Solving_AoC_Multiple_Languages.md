---
date: 2025-02-03 22:49:00
description: Measuring Gemini's ability to solve Advent of Code Puzzles in Multiple Languages
tags: advent_of_code, gemini, python, vs_code, C, C#, C++, Clojure, Common Lisp, Dart, F#, Go, Haskell, Java, JavaScript, Kotlin, Lua, Objective-C, OCaml, Perl, PHP, Python, Ruby, Rust, Smalltalk, Swift, TypeScript, Zig
title: Using a Gemini Thinking Model to Solve Advent of Code 2024 Puzzles in Multiple Languages
---

The Advent of Code is a yearly programming contest made up of Christmas-themed puzzles designed to be solved using any programming language. I measured how well the gemini-2.0-flash-thinking-exp-01-21 LLM solves the 2024 contest puzzles, using a variety of popular programming languages: C, C#, C++, Clojure, Common Lisp, Dart, F#, Go, Haskell, Java, JavaScript, Kotlin, Lua, Objective-C, OCaml, Perl, PHP, Python, Ruby, Rust, Smalltalk, Swift, TypeScript, and Zig

I developed a prompt was developed to guide the model in a multi-turn conversation. The model was given 5 conversational turns to produce a correct puzzle solution.

Results: 24 popular programming languages were measured. Depending on the programming language used, the model was capable of solving from a high of 69% to a low of 8% of the puzzles.

The model optionally supports code execution of Python programs. Enabling code execution results in poorer performance. The solve rate with no code execution: 69%, code execution of examples: 59%, code execution of examples and the actual puzzle input: 16%.

Here's the full paper: [Using a Gemini Thinking Model to Solve Advent of Code 2024 Puzzles in Multiple Languages](https://github.com/jackpal/publications/blob/main/aoc2024/paper.md)
