---
date: 2021-12-22
description: Notes on participating in the Advent of Code programming puzzle contest.
tags: advent_of_code programming_contests Python Swift
title: The Zen of Advent of Code
---

For the past three years I've been participating in the
[Advent of Code](https://adventofcode.com/) programming puzzle contest. It's a free contest that
has run every December since 2015. It's appropriate for people who can write programs at the
undergraduate college student level. (Which means that many high school
students can do it.)

The way the contest works is that, starting at midnight East Coast time on the morning of
December 1st, a puzzle is announced every day from December 1st to December 25th.

The puzzles are designed to be solved by an ordinary developer within a few hours. The focus
tends to be on figuring out a good algorithm, and the problems are usually solvable using under a
hundred lines of code in standard Python.

Although Python is the focus of the contest, you can use any language you want, and for the most
part the problems can be solved using any language. The typical pitfals of using a language
besides Python are:

  + Some problems require processing large integers. You may need to find or implement a "BigNum"
    libray for your language. (Although usually 64 bit ints are enough.)

  + Some problems are easily solved by using standard data structures (like a priority queue)
    that you will have to find or implement for your language.

Each year's puzzles start out easy, and typically get harder over the days leading up to December
24th. December 25th's puzzle is usually easy. Each puzzle comes in
two parts. The first part is usually easy, to make sure you understand how to parse the puzzle input and that you understand the general nature of the puzzle. The
second part is usually a twist on the first part, that makes the puzzle harder to solve. Especially in later days, you may find that your solution to the first
part has to be rewritten to solve the second part.

In early years of the contest the puzzles were themed around Christmas. Over the years this focus
has faded, and in recent years Christmas barely appears beyond a few token mentions of "elves".
I miss the Christmas theme, but I think the puzzle creator ran out of Christmas-related jokes and
ideas.

Anyway, here are my tips for having an enjoyable time participating in the contest:

1. Depending on where you live, the puzzles may be released the day before their date. For
   example, on the West Coast of the US, the first day's puzzle will becom available at
   9 pm on November 30th.

2. Don't worry about your leaderboard position. I myself have never placed higher than 490 on
   any part of any problem, but I usually place in the 1000s-3000s, and I still enjoy
   participating.

3. You can save a few minutes each day by starting your IDE and creating empty functions and
   files for the day's problems before the contest starts for the day.

4. Read the problem description fully. Make sure you're solving the problem they ask you to
   solve, and not some similar or harder problem. I've occasionally wasted hours solving a
   general case when the problem only required solving an easier specific version of the problem.

5. If you find yourself writing more than 100 lines of code, or taking more than an hour, you
   probably haven't figured out the optimal way of solving the problem. All the problems so far
  have been solvable in normal Python, using normal data structures like list, dictionary, set,
  and tuple.

6. That being said, a few Python libraries and data structures have proven helpful in many
   problems:
    + Counter
    + heapq
    + deque

7. Python-specific advice:
    + Don't bother with regular expressions. Unless you know regular expression like the back of your hand, it's almost always faster and easier to parse the
      puzzle input using "split" and "int".
    + Intentionally avoid programming practices that are good for larger programs, because they will slow you down for tiny programs like these:
        - Don't parse the input into an intermediate data structure. Instead, parse as you go about solving the puzzle.
        - For the most part, don't define subroutines.
        - For the most part, don't define classes. You can get really far with tuples, lists, sets and dicts.
        - Use short variable names.
        - Use "for" loops instead of comprehensions.
    + Generally you won't need numpy or related libraries.
      - The puzzle author is aware of scipy, numpy, networkx, etc, and tends to add "twists" to the puzzle that make it difficult to solve the puzzle simply by
        calling a pre-built function of one of these libraries.
    + Python plotting libraries are useful for making visualizations of your data for debugging and or creating Redit posts or blog posts.

8. Swift-specific advice:
    - Swift string processing is verbose. Do yourself a favor and write Swift versions of Python's "split", "join", and enumerate-by-character functions before the
      contest starts.
    - Swift lacks some useful collection classes. Write or find your own version of priority queue and counted set.
    - Swift's fancy map/reduce methods work, but simple for loops are often faster and easier to write.

9. Many years have had problems related to the following topics, so it's worth reading about them
   ahead of time:
    + Chinese Remainder Theorem.
    + Ring buffers
    + Searching graphs for optimal routes.
    + Cellular automata.
    + One year had many problems related to writing an interpreter for a simple computer. However, this was so difficult for many contestants that I am not sure
      we'll see any more puzzles on this topic. 

10. The Reddit community [r/adventofcode](https://www.reddit.com/r/adventofcode/) is a wonderful
    resource of support and ideas related to the contest.

    - If you get stuck on a problem, I recommend visiting the corresponding r/adventofcode solution thread and reading through the highest-rated solutions.
      - There is no shame in doing this. Some of the problems are very difficult to solve if you're not familiar with a particular obscure algorithm or technique.

11. Try finding someone to be a contest buddy. In the past two years I've been doing the contest
    with my kids (who are high school and college age).
    It's been fun discussing the problems and comparing implementations. My son's recently gotten faster than me at solving the problems. I try to be
    philisophical about being surpassed.

