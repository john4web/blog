+++
title = "Elasticsearch Basics"
date = "2024-01-24"
description = ""
+++

## What is Elasticsearch?

Elasticsearch is a search engine written in java. You can think of it as a very fast/performant database, which is specialized on search capabilities (full-text search) and indexing large volumes of data. It allows users to perform complex queries, filter and aggregate data

## Why using Elasticsearch?

Why do people use Elasticsearch at all instead of a relational database like MySQL? Because Elasticsearch is much more performant and optimized for search functionalities. Typically, data is "copied" from a database into an Elasticsearch index because it is much faster and more performant when accessed there. This high performance is achieved, among other things, through the "inverted index." You can then communicate with this Elasticsearch index via a REST interface, which is precisely optimized for tasks like "searching" in applications. However Elasticsearch has several other advantages.

## Documents, Indices and clusters

When thinking about a relational database schema, we have databases, tables and rows. In Elasticsearch we kinda have the same concept. However in Elasticsearch they are called **clusters, indices and documents**.

### cluster
An ElasticSearch Cluster is the equivalent to a Database in SQL.

### indices
Indices are like tables in SQL. An index powers search into all documents within a collection of types. They contain inverted indices that let you search across everything within them at once, and mappings that define schemas for the data within.

### documents
Documents are like records in an SQL table. Documents are the things you're searching for. They can be more than text – any structured JSON data works. Every document has a unique ID, and a type.

The schema for your documents are defined by the index respectively the document type associated by the index.

## Inverted Index
Elasticsearch's high performance is largely due to its use of an inverted index. An inverted index is a data structure used to enable fast full-text searches. It maps terms to the documents that contain them, allowing Elasticsearch to quickly retrieve and rank documents based on search queries. This is particularly efficient for searching large volumes of data, as the inverted index allows Elasticsearch to quickly locate and retrieve the relevant documents without scanning the entire dataset.

__"The inverted index quickly maps search terms to documents"__

How does this work? Let's take a look at an example. In the following, you can see two documents with some text in it:

- **Document 1:** _Space: The final frontier. These are the voyages..._
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

## Relevance

How to deal with the concept of relevance?
TF-IDF means Term Frequency * Inverse Document Frequency

Term Frequency is how often a term appears in a given document. If the term space appears very frequently in a given document, then it would have a high term frequency.

Document Frequency is how often a term appears in all documents in your entire index

If you divide the Term Frequency by the Document Frequency, you get the relevance of a term in a document (= we see how special this term is to the document).

This is how search engines work. If you search for a given term, it will try to give you back results in the order of their relevance.

## ElasticSearch's REST Interface
You can communicate with Elasticsearch via REST i.e. (GET, POST, PUT, DELETE).

```bash
curl -XGET 127.0.0.1:9200/shakespeare/_search?pretty -d
'{
    "query":{
        "match_phrase":{
            "text_entry": "to be or not to be"
        }
    }

}'
```
In this example, we are querying the Shakespeare index for the phrase "to be or not to be".
"_search" means that we wanna search and "pretty" means that the result should be formatted.


```bash
curl -XPUT 127.0.0.1:9200/movies/movie/109487 -d
'{
    "genre": ["IMAX", "Sci-Fi"],
    "title": "Interstellar",
    "year": 2014

}'
```
In this example we are inserting data into the index.
"-d" -> specifies the data that should be inserted  
movies -> this is the index name  
movie -> this is the data type  
109487 -> this is the unique identifier  

## Mapping

A mapping is a schema definition. ElasticSearch has reasonable defaults, but sometimes you need to customize them. Mappings tell elasticsearch "how" to store the information. It tells elasticsearch what format to store the data in and how to index it and how to analyze it.

Example of importing some movie data into elasticsearch where we want the year to be explicitely interpreted as a date-type field.

```bash
curl -XPUT 127.0.0.1:9200/movies/ -d
'{
    "mappings": {
        "proterties": {
            "year": {
                "type": "date"
            }
        }
    }
}'
```

You can get the mapping information of an Index with the following command:

```bash
curl -XGET "http://127.0.0.1:9200/demo-default/_mapping?pretty=true"
```

This gets the mapping information of an Index called "demo-default".



mappings can define different things:

### Field types
These are types, that elastic search recognizes: string, byte, short, integer, long, float, double, boolean, date

```json
    "proterties": {
        "user_id": {
            "type": "long"
        }
    }
```

### Field index
Here you can specify whether you want this field indexed for full-text search or not.
If you specify ``"index": "not_analyzed"``, then it won't be found.
``"index": "analyzed"``and ``"index": "no"`` is also possible.

```json
    "proterties": {
        "genre": {
            "index": "not_analyzed"
        }
    }
```

### Field Analyzer

Here you can define your tokenizer and token filter.

You can specify the language for instance:
```json
    "proterties": {
        "description": {
            "analyzer": "english"
        }
    }
```

There are three things an Analyzer can do:

1. **Character Filters:** remove HTML encoding, convert ``&`` to ``and``
for example: if someone searches for ``and`` or someone searches for ``&``, then in bot cases, the result with ``&`` will be returned from the search. For example "Kaffee&Kuchen".

2. **Tokenizer:** split strings up in a certain way. 
You can define what is a word by splitting strings up by whitespace or punctuation or non-letters, etc.

3. **Token Filter:** can do lowercasing, stemming, synonyms and stopwords

**Lowercasing:** The token filter converts everything to lowercase. When you want to do a case-insensitive search, then you can use that.

**Stemming:** When you search for box, then the search should also return all terms that contain the following: boxes, boxing, boxed, box

**Synonyms:** If you search for "big", then the search should also return "large" in the result set.

**Stopwords:** If you want to save space and avoid storing insignificant words like "and," "the," "a," etc. These words don't carry much information and are therefore not as important. However, there are also situations where you definately don't want to do that. So think wisely if you want to exclude such stopwords. Carefully consider whether you need them in your search. Stopwords can sometimes lead to unintended side effects.

### Choices for analyzers:

There are some predefined analyzers that you can use:

**Standard**
Splits on word boundaries, removes punctuation, lowercases. Good choice if language is unknown.

**Simple**
Splits on anything that isn't a letter, and lowercase

**Whitespace**
Splits on whitespace but doesn't lowercase

**Language (i.e. english)**
Accounts for language-specific stopwords and stemming



## Reference

Frank Kane: Elasticsearch 8 and the Elastic Stack: In-Depth and Hands-On
https://www.oreilly.com/library/view/elasticsearch-8-and/9781788995122/ 