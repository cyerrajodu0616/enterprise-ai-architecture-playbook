# Architect's Reflection — Day 1

## What surprised me?

The problem is not RAG. It is authority, freshness, correctness, access pattern, and business risk.

## Which assumption did I challenge?

That enterprise AI data should be centralized in a vector database.

## What would I still be uncomfortable defending?

A final long-context-vs-retrieval decision without corpus and query evidence.

## What evidence would I require?

Question taxonomy, source-of-truth map, version semantics, freshness, latency, source capacity, authorization, and evaluation thresholds.

## Could the design be simpler?

Yes. Direct context or structured access may remove the need for retrieval entirely for some question classes.
