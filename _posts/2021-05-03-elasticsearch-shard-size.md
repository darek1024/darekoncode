---
title: 'Elasticsearch Perfect Shard Size'
date: 2021-05-03 07:30:00
---

Documents in Elaticsearch are logically grouped into a data structure called index. One index can contain a massive number of documents.

You can think of an index like a database table and a document like a database row.

But, luckily Elasticsearch is built around distributed concepts, so a single index (table) can be split into multiple physical locations by storing documents in multiple shards. One index is built of multiple shards which contain documents. All shards together make up a single index.

Now a substantial problem arises. You can choose the number of shards only once at the beginning of index creation. Depending on the number of shards, Elasticsearch knows where to assign specific documents on writes and find them on reads.

So if there are three shards, the location is calculated by hashing function and modulo of three. If you were allowed to change the shards number later, the modulo function of four would produce incorrect results.

## So, how many shards should the index have?

Quite often, techs working with Elasticsearch ponder how many shards a single index should have. Two, three, four, or... a hundred?

And the answer is: it is a wrong question :)

## The correct question: how large should a single shard be?

So the answer is: **a single shard should be of size 5 to 40 GB.**

Depending on the number of documents you plan to store in your index and their size, you can plan the number of shards for the index.

**If you intend to have 1 GB of data, it's enough to have just one shard :)**

**But if you have 100 GB of data, it's better to have 3-20 shards.**

The specific number depends on your performance benchmarks.

The more shards, the better it is to scale them out on multiple nodes. The less, the latency on communication is lower (calculations are performed locally).

## What if I'm not sure how much data I am going to have?

The answer is: start small. If the need for more shards arises, you can always reindex the data on the index with a more precise number of shards later on.

## Wrap Up 🙌
Choosing the correct number of shards for an Elasticsearch index is one of the most important decisions to make at the beginning. Make sure you use the correct numbers, so you gain the best performance experience for your cluster.
