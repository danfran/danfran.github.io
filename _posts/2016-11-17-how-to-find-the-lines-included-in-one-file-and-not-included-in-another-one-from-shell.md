---
layout: post
title: "How to Find the Lines Included in One File and Not Included in Another One From Shell"
date:   2016-11-17
categories: shell
tags: cat grep
---
```sh
$ cat a.csv
aaa
bbb
ccc
$ cat b.csv
bbb
aaa
ddd
eee
$ grep -Fvf a.csv b.csv
ddd
eee
$ grep -Fvf b.csv a.csv
ccc
```
