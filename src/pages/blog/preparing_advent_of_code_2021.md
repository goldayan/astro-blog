---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Advent of Code Preparations
description: advent of code preparation
date: 2021-11-29
author: "Gold Ayan"
category: "Coding"
tags:
  - aoc
---

Advent of code starts at December 1 every year and it will continue till
December 25. Each day you will get 2 problems to solve.

site: [https://adventofcode.com/](https://adventofcode.com/)

What can you learn in Advent of code ?

- Programming challenges will be like a story to save santas which is cute.
- It touches Algorithms, Data Structure, Graph, Regex and lot more.
- It is one of the cool way to learn or enhance your skills in programming language.

<!-- more -->

I tried advent of code in following years but i am able to complete Day 4
problems in 2019 and Day 7 in 2020 only. This year my goal is to complete all 25 Day
challenges and try to solve it using TDD(Test Driven Development) style.

## TDD style

- Write test
- Write Code
- If Test fails, Refactor the code
- Repeat till you get final solution

### More on TDD

- Book: Test Driven Development By Example - Kent Beck.
- [https://kentbeck.github.io/TestDesiderata/](https://kentbeck.github.io/TestDesiderata/)

## Preparation the environment

- Pick your programming language
  - Me: Swift.
- Start solving problems.
- Automate directory structure creation. (optional)

### Automate Directory Creation

- Create project structure for each day.
- This save me few mintues.

```sh

#!/bin/sh

echo "Which Day challenger ?"
read DAY

DIRPATH="Day $DAY"
mkdir "$DIRPATH"
cd "$DIRPATH"

swift package init

# I prefare library if you prepare executable file use below command
#swift package init --type=executable

# If you prefare solving in XCode, uncomment next line.
# swift package generate-xcodeproj

```

Let see how it goes.

If you like to get along and start solving challenges, you are all welcome. If
you manage to complete all challenge. I wil present you a small prize (First Come First Serve)
