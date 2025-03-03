---
id: search.md
summary: Conduct a vector similarity search with Milvus.
---

# Conduct a Vector Similarity Search
This page will show you how to conduct a similarity search in Milvus.

1. Create search parameters:

{{fragments/multiple_code.md}}

```python
>>> search_params = {"metric_type": "L2", "params": {"nprobe": 10}}
```

```javascript
const searchParams = {
  anns_field: "example_field",
  topk: "4",
  metric_type: "L2",
  params: JSON.stringify({ nprobe: 10 }),
};
```

2. Load the collection to memory before conducting a vector similarity search:

{{fragments/multiple_code.md}}

```python
>>> collection.load()
```

```javascript
await milvusClient.collectionManager.loadCollection({
  collection_name: COLLECTION_NAME,
});
```

3. Call `search()` with the newly created random vectors `query_records`:

_Milvus returns the IDs of the most similar vectors and their distances._

{{fragments/multiple_code.md}}

```python
>>> results = collection.search(vectors[:5], field_name, param=search_params, limit=10, expr=None)
>>> results[0].ids
[424363819726212428, 424363819726212436, ...]
>>> results[0].distances
[0.0, 1.0862197875976562, 1.1029295921325684, ...]
```

```javascript
await milvusClient.dataManager.search({
  collection_name: COLLECTION_NAME,
  // partition_names: [],
  expr: "",
  vectors: [[1, 2, 3, 4, 5, 6, 7, 8]],
  search_params: searchParams,
  vector_type: 100, // Float vector -> 100
});
```

To search in a specific partition or field, set the parameters `partition_names` and fields when calling `search()`.

{{fragments/multiple_code.md}}

```python
>>> collection.search(vectors[:5], field_name, param=search_params, limit=10, expr=None, partition_names=[partition_name])
```

```javascript
await milvusClient.dataManager.search({
  collection_name: COLLECTION_NAME,
  partition_names: [partition_name],
  expr: "",
  vectors: [[1, 2, 3, 4, 5, 6, 7, 8]],
  search_params: searchParams,
  vector_type: 100, // Float vector -> 100
});
```

4. Release the collections loaded in Milvus to reduce memory consumption when the search is completed. Query other collections:

{{fragments/multiple_code.md}}

```python
>>> collection.release()
```

```javascript
await milvusClient.collectionManager.releaseCollection({
  collection_name: COLLECTION_NAME,
});
```
