---
title: Payload
weight: 25
---

One of the key features of Qdrant is the ability to store additional values along with vectors.
These values are called `payload` in Qdrant terminology.

Payload is a set of key-value data. Each key can have several values of the same type.

Here is an example of a typical payload represented in json:

```json
{
    "colors": ["red", "blue"],
    "price": 11.99,
    "locations": [
        {
            "lon": 52.5200, 
            "lat": 13.4050
        }
    ]
}
```

Qdrant will try to automatically recognize the value type for each key, but you can also specify it explicitly:

```json
{
    "colors": {
        "type": "keyword",
        "value": ["red", "blue"] 
    },
    "price": {
        "type": "float",
        "value": 11.99
    },
    "locations": {
        "type": "geo",
        "value": [
            {
                "lon": 52.5200, 
                "lat": 13.4050
            }
        ]
    }
}
```

Here are the types of value that are currently available in Qdrant:

* `integer` - 64-bit integer in the range `-9223372036854775808` to `9223372036854775807`.
* `float` - 64-bit floating point number.
* `keyword` - string value.
* `geo` - Geographical coordinates. Example: `{ "lon": 52.5200, "lat": 13.4050 }`

Values corresponding to the same key in different records must have the same type.

In other words, it is impossible to have one vector associated with payload `{"price": 11.99}`, and another vector associated with `{"price": "cheap"}`.
To maintain type consistency, Qdrant has a payload schema associated with the collection.
This schema is available at the [collection info API](https://qdrant.github.io/qdrant/redoc/index.html#operation/get_collection)

## Payload filtered search

Qdrant not only stores payload along with vectors, but also allows you to search based on its values.
This feature is implemented as additional filters during the search and allows you to incorporate custom logic on top of semantic similarity.

The filtering process is discussed in detail in the section [Filtering](../filtering).


## Create point with payload

With REST API

```bash
```

With python

```python
```

## Update payload

With REST API

```bash
```

With Python client

```python
```

## Payload indexing

In order to search more efficiently with filters, Qdrant allows you to specify payload fields as indexed.
For marked fields will Qdrant will build an index for the corresponding types of queries.

The indexed fields also affect the vector index, see [Indexing](../indexing) for details.

In practice, we recommend creating an index on those fields that could potentially constrain the results the most.
For example, building an index for the object ID (if it is actually used in the filter) will be much more efficient than an index by its color, which has only a few possible values.

In the case of compound queries involving multiple fields, Qdrant will attempt to use the most restrictive index first.

To mark a field as indexable, you can use the following code.

With REST API

```json
```

With Python client

```python
```

The index usage flag is displayed in the payload schema with the [collection info API](https://qdrant.github.io/qdrant/redoc/index.html#operation/get_collection).

Payload schema example:

```json
{
    "payload_schema": {
        "property1": {
            "data_type": {
                "type": "keyword"
            },
            "indexed": true
        },
        "property2": {
            "data_type": {
                "type": "integer"
            },
            "indexed": false
        }
    }
}
```