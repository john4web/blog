+++
title = "Elasticsearch Basics"
date = "2024-01-24"
description = ""
+++

## What is Elasticsearch?

Elasticsearch is a search engine written in java. You can think of it as a very fast/performant database, which is specialized on search capabilities (full-text search) and indexing large volumes of data. It allows users to perform complex queries, filter and aggregate data

## Why using Elasticsearch?

Why do people use Elasticsearch at all instead of a relational database like MySQL? Because Elasticsearch is much more performant and optimized for search functionalities. Typically, data is "copied" from a database into an Elasticsearch index because it is much faster and more performant when accessed there. This high performance is achieved, among other things, through the "inverted index." You can then communicate with this Elasticsearch index via a REST interface, which is precisely optimized for tasks like "searching" in applications. However Elasticsearch has several other advantages.

## Documents and Indices

When thinking about a relational database schema, we have tables and rows. In Elasticsearch we kinda have the same concept. However in Elasticsearch they are called **indices and documents**.

### indices
Indices are like tables in SQL. An index powers search into all documents within a collection of types. They contain inverted indices that let you search across everything within them at once, and mappings that define schemas for the data within.

### documents
Documents are like records in an SQL table. Documents are the things you're searching for. They can be more than text – any structured JSON data works. Every document has a unique ID, and a type.

## Inverted Index
Elasticsearch's high performance is largely due to its use of an inverted index. An inverted index is a data structure used to enable fast full-text searches. It maps terms to the documents that contain them, allowing Elasticsearch to quickly retrieve and rank documents based on search queries. This is particularly efficient for searching large volumes of data, as the inverted index allows Elasticsearch to quickly locate and retrieve the relevant documents without scanning the entire dataset.

__"The inverted index quickly maps search terms to documents"__

How does this work? Let's take a look at an example. In the following, you can see two documents with some text in it:

- **Document 1:** _The final frontier. These are the voyages..._
- **Document 2:** _He's bad, he's number one. He's the space cowboy with the laser gun!_

Elasticsearch now takes a look at all the search terms and "remembers", in which documents they occur. As shown in the following table:

| Search Term  | Documents |
| ----- | --- |
| space   | 1, 2  |
| the | 1, 2  |
final |1 |
frontier | 1 |
he | 2 |
bad |2 |
... | ...

## ElasticSearch's REST Interface
You can communicate with Elasticsearch via REST i.e. (GET, POST, PUT, DELETE).



## Reference

Frank Kane: Elasticsearch 8 and the Elastic Stack: In-Depth and Hands-On
https://www.oreilly.com/library/view/elasticsearch-8-and/9781788995122/ 