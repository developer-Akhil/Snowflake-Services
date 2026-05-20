**EMBED_TEXT_768/1024()**

In Snowflake Cortex AI, EMBED_TEXT_768 and EMBED_TEXT_1024 are embedding models/functions used to convert text into vector embeddings for AI search, semantic similarity, and RAG (Retrieval-Augmented Generation) applications.

These embeddings transform text into numerical vector representations that AI systems can understand semantically.

EMBED_TEXT_768 and EMBED_TEXT_1024 are built-in Snowflake Cortex AI SQL functions used to convert plain text into numerical vector representations. These vectors are then used for semantic search, clustering, or Retrieval-Augmented Generation (RAG) pipelines.


**The Difference: 768 vs. 1024**

The numbers refer to the dimensions (or length) of the resulting vector arrays:

•	EMBED_TEXT_768: Returns a 768-dimensional vector. It typically utilizes models like snowflake-arctic-embed-m or e5-base-v2. This dimension offers a strong balance between capturing semantic meaning and optimizing storage/computation.
```
SELECT SNOWFLAKE.CORTEX.EMBED_TEXT_768('snowflake-arctic-embed-m-v1.5', 'Your text goes here') AS vector_embedding;
```
•	EMBED_TEXT_1024: Returns a 1024-dimensional vector, which allows for capturing deeper, more granular linguistic nuances, though it requires slightly more storage and compute to process.
```
SELECT SNOWFLAKE.CORTEX.EMBED_TEXT_1024(
    'e5-base-v2',
    'Explain vector embeddings'
);
```
