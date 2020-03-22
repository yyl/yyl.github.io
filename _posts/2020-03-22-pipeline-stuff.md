---
layout: post
title:  "Pipeline stuff"
date:   2020-03-22 22:23:15
categories: Engineering
---

Just old stuff for reference.

- MapReduce is a programming model to compute/process large scale of data in distributed and parallel way
    - mapper: read data from each node, compute them into K/V pairs
    - reducer: shuffle pairs based on the key, and combine them back (“grouped by key”)
- Hadoop is open-source framework based on MapReduce
- HDFS is the underlying distributed file system to store large scale of data
- Spark is another framework built on top of MapReduce
    - core is can do in-memory computing thus way faster

### Spark basics

- no thrift support, it uses RDD
- laziness compute: intermediate RDD transformations won't run
    - transformation vs. action
- ona master, one manager, many workers
- basic data types: RDD, DataSet, DataFrame