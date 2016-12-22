---
layout: post
title: "Read and Filter the Lines of a Text File and Save the Output to Another File with Haskell"
date: 2016-12-22
categories: haskell
tags: file filter
---
Read the content of the file `input.txt`, split it in multiple lines `\n` via `lines`, apply a `filter` and save it back:

```haskell
module Main where

import Data.Char

main :: IO ()
main = do
    fileContent <- readFile "input.txt"
    let validWords = unlines $ filter isValidWord $ lines fileContent
    writeFile "output.txt" validWords

isValidWord w = length w > 5 && all isAlpha w
```
