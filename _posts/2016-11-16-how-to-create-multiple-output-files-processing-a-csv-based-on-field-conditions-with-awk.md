---
layout: post
title: "How to Create Multiple Output Files Processing a CSV Based on Field Conditions with AWK"
date:   2016-11-16
categories: shell
tags: awk
---
```sh
awk -F\, '$8 == "\"Some value\"" {
    gsub(/"| /, "", $6);

    if ($4 ~ /foo/)
    {
      print > "output1.csv"
    } else if ($4 ~ /bar/)
    {
      print > "output2.csv"
    } else
    {
      print > "output3.csv"
    }
  }' input_file.csv
```
