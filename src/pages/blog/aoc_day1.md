---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: AOC2022 Day1
description: Advent of code 2022, Day 1: Calorie Counting
date: 2022-12-02
author: "Gold Ayan"
category: "Coding"
tags:
  - AOC
---

Sample input for the Day 1 (input.txt)
```
1000
2000
3000

4000

5000
6000

7000
8000
9000

10000
```

### Part 1
- We need to add all the numbers until there is empty line and find out which is the max value.
```clojure
(defn part1 [file]
  
  (let [input (clojure.string/split-lines (slurp file))
        max-number (atom 0)]

  (loop [counter 0 
         elements input]
    (let [element (first elements)]
      (if (nil? element)
        @max-number
        (if (clojure.string/blank? element)
          (do (swap! max-number max counter)
              (recur 0 (rest elements)))
          (recur (+ counter (Integer/parseInt element)) (rest elements))))))))

(part1 "input.txt")
```
- I loop through the element and add it to counter.
- when there is empty line, choose the max(max-number, counter) and reset the counter.
- None (first elements) is nil we return the max-number

### Part 2
- We need to add all the numbers until there is empty line and find out which is the first 3 max value and add them together.
```clojure
(defn part2 [file]
  
  (let [input (clojure.string/split-lines (slurp file))
        max-number (atom [])]

  (loop [counter 0 
         elements input]
    (let [element (first elements)]
      (if (nil? element)
        @max-number
        (if (clojure.string/blank? element)
          (do (swap! max-number conj counter)
              (recur 0 (rest elements)))
          (recur (+ counter (Integer/parseInt element)) (rest elements))))))))

(apply + (take 3 (sort > (part2 "input.txt"))))
```
- I loop through the element and add it to counter.
- when there is empty line, append to the max-number list and reset the counter.
- None (first elements) is nil we return the max-number list
- sort the list in descending order, take 3 element and apply sum on those

### Learned from others
- `(str/split input #"\n\n")` we can use \n\n to take the content easily.
- `(take-last 3 (sort <Collection>))` Another way to take 3 max numbers from collection.
- `(parse-long "56")` convert string to number in clojure


### Reference
- https://github.com/Teekeks/AoC2022/blob/master/src/de/teawork/aoc2022/day/01.clj
  - I learned about new library `Fluokitten` in this repo (category theory in clojure).
- https://github.com/vollcheck/aoc/blob/master/src/y22/day01.clj
- https://github.com/Akallabet/aoc-clojure/blob/main/day-1/day-1.clj
