---
title: 'Elasticsearch Perfect Shard Size'
date: 2021-05-03 07:30:00
---

One of the most common question when working with Elasticsearch is how to choose correct shards number of the index.

The question is important because it is not possible to adjust that number later. It can be set only at the beginning of the index creation.

So one must choose the number of shards carefully.

And what is important the ideal number of shards depends on the perfect size of a single shard.

You can have multiple options.

* Option 1 - many small shards, so they can easily be spread out on nodes of your cluster

* Option 2 - as biggest shards as possible, so all the data fit into one node, making calculations faster.

## Perfect shard should sized between 5-40 GB.

The actual number should be picked depending on benchmarks, but it probably sits between 5 and 40 GB.

The answer comes from the Elastic official trainers from official Elastic training program.

So, what to take from this entry?

* if your shards are sized for 500 MB, and you have many of them, you can safely merge them to bigger ones, to fit between 5 to 40 GB
* if your shards are bigger than 40 GB, it may cause performance problems and slows downs, and in that case you'd better reindex data to smaller ones.

Remember, that even if you don't know expected size of data in the index you can always reindex the data to the new one. So you can start small - for instance 2-5 shards at the beginning, and then adjust that accordingly by reindexing data once again.

It can cause troubles to maintain writing & reading from two indexes at the same time, but this problem we will cover in another blogpost.

## Wrap Up

When it comes to adjusting size of a shard in Elasticsearch index one must act carefully. Choosing too many small shards may cause to much latency in communication, and not providing the benefit of better scalability at the beginning. It's ok to plan shards number for capacity of 5 to 40 GB, and if need be adjust the size later by reindexing the data again.