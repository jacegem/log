---
date: 2024-12-01
tags:
- Advent of Code
- AOC
title: Advent of Code
categories:
lastMod: 2025-01-03
---




## 2024

[Advent of Code/2024/day01]()





```clojure
(ns advent-of-code.core
  (:require
   [clojure.java.io :as io]
   [clojure.string :refer [split-lines trim-newline]]))

(defn read-file [path]
  (-> path
      io/resource
      slurp
      trim-newline
      split-lines))

(defn read-lines [year day]
  (-> (format "advent_of_code/%d/day_%02d.txt" year day)
      read-file))

(defn read-numbers [year day]
  (->> (read-lines year day)
       (map parse-long)))

```






