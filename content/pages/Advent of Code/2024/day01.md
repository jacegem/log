---
tags:
- clojure
- Advent of Code
- aoc
- 2024
- day01
date: 2024-12-03
categories:
- clojure
- aoc
- Advent of Code
link: https://adventofcode.com/2024/day/1
hugo-title: aoc 2024 by hugo
title: Advent of Code/2024/day01
lastMod: 2025-01-03
---


https://adventofcode.com/2024/day/1

## [Advent of Code]({{< ref "/pages/Advent of Code" >}})





왼쪽 목록과 오른쪽 목록을 각각 정렬하고, 순서대로 거리를 구하고 모두 합한다.

```shell
3   4
4   3
2   5
1   3
3   9
3   3
```



정렬하면, 이렇게 나오고,

```clojure
{:left (1 2 3 3 3 4), 
 :right (3 3 3 4 5 9)}
```



각각의 거리를 더하면, 11 이 된다.

`2 + 1 + 0 + 1 + 2 + 5` = 11



```clojure
(ns advent-of-code.2024.day-01
  "https://adventofcode.com/2024/day/1
   --- Day 1: Historian Hysteria ---"
  (:require
   [advent-of-code.core :refer [read-lines]]))

(def sample ["3 4"
             "4 3"
             "2 5"
             "1 3"
             "3 9"
             "3 3"])

(defn sort-numbers [lines]
  (-> (reduce (fn [acc row]
                (let [[left right] (map parse-long (re-seq #"\d+" row))]
                  (-> acc
                      (update :left conj left)
                      (update :right conj right))))
              {:left  []
               :right []}
              lines)
      (update :left sort)
      (update :right sort)))

(defn solve [lines]
  (let [numbers              (sort-numbers lines)
        {:keys [left right]} numbers]
    (->> (map vector left right)
         (map (fn [[l r]] (abs (- l r))))
         (apply +))))
```






