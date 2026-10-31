Weaviate Embeddings handles all embedding generation server-side; no local GPU or model downloads required. Simply upload document images as base64 blobs and Weaviate generates multi-vector embeddings automatically.
- `weaviate-client`: Python client for Weaviate vector database (v4.x); Weaviate Embeddings handles all embedding generation
- **`doc_page`**: BLOB property that holds base64-encoded page images; the vectorizer reads this field
- **`model`**: `ModernVBERT/colmodernvbert`; 250M parameter late-interaction vision-language encoder, fine-tuned for visual document retrieval
Weaviate Embeddings generates embeddings server-side during import; no local model needed:
        # Add object to batch: Weaviate generates embeddings automatically
Weaviate handles query embedding automatically; just pass text:
    # Search: Weaviate embeds the query server-side
- Reduce `num_documents`; retrieve fewer documents (even 1 can work well)
- Reduce `max_tokens`; shorter responses use less memory