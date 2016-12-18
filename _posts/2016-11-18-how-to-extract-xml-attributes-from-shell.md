---
layout: post
title: "How to Extract XML Attributes from Shell"
date:   2016-11-18
categories: shell sh grep echo
---
### How to Extract XML Attributes from Shell
```sh
$ echo '<mydata att1="foo" att2="bar" att3="ter" />' | grep -oPm1 "(?<=att2=\")[^\"]+"
bar
```
