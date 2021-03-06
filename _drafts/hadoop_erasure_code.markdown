---
title: "Erasure Coding in Hadoop"
layout: post
image: /assets/images/markdown.jpg
headerImage: false
tag:
- hadoop
- data science
star: false
category: blog
author: matthewroseman
description: How hadoop replaces replication with erasure code
---

## Summary:

With release 3.0 the Hadoop file system (HDFS) uses erasure coding as a replacement for replication in ensuring the
durability of stored data. Instead of replicating the data 3x times, erasure coding uses error correcting algorithms so
that if any nodes are lost, the data should be salvageable.

## Overview:

Using erasure coding requires that the way files are stored changes. A method called stripping is used which divides up
the data into units and stores consecutive units on seperate disks.
![Stripping]({{site.url}}/assets/images/stripping.png)
For each stripe parity data is calulated using erasure coding, and they are stored on a seperate disk.

With this setup, if any node in a stripe is lost, as long as the parity machines remain, and a certain percent of a stripes data is
still up, the data can be recovered.

Before, if you had 6 blocks of data, you would have 3x replication so 6*3=18 blocks of disk space, but with erasure
coding, you just need 3 blocks for parity data, so 6+3=9 blocks of disk space.

## Algorithm:

An erasure code is a forward error correctin code, that transforms a message of length **k** into a longer message with **n**
*"code word"* symbols, such that the original message can be recovered from a subset of the **n** *"code word"* symbols.

The specific erasure code algorithm HDFS uses is Reed-Solomon Error Correction. This algorithm takes k data symbols of s
bits each, and adds parity symbols to make n symbol code words. A RS decoder can correct up to t symbols, where 2t =
n-k.
![Code Word]({{site.url}}/assets/images/reed_solomon_code_word.gif)

For more information about Reed-Solomon I wrote
