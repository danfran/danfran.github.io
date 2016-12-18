---
layout: post
title: "How to Extract XML Attributes from Shell"
date:   2016-11-18
categories: shell
tags: grep echo
---
```sh
$ echo '<mydata att1="foo" att2="bar" att3="ter" />' | grep -oPm1 "(?<=att2=\")[^\"]+"
bar
```
