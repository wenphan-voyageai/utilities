# Batch Embed

If you want to create embeddings in batch, you need to consider [API max batch size and tokens](https://docs.voyageai.com/reference/embeddings-api).  The following is example Python code for creating embeddings in batch.  It assumes you have properly [installed the Voyage AI python package, have a configured API key](https://docs.voyageai.com/docs/api-key-and-installation), and your documents are in a list named `documents`.

The batch size limit (`batch_size`) is set to 128.  The code also prints out useful runtime metadata: total documents, progress on documents being embedded, and total tokens for the batch.

```python
import voyageai

vo = voyageai.Client()

# Assumes documents are in a list called `documents`.

# Embed more than 128 documents
# you can use a for loop to embed them.
batch_size = 128
print("Total documents: " + str(len(documents)))

for i in range(0, len(documents), batch_size):
        current_doc_batch = documents[i:i + batch_size]

        # Get actual last index
        if (i + batch_size >= len(documents)):
                i_end = len(documents)
        else:
                i_end = i + batch_size - 1

        print("Embedding documents " + str(i) + " to " + str(i_end) + ".")
        print("Total tokens: " + str(vo.count_tokens(current_doc_batch)))

        documents_embeddings = [
                vo.embed(
                        current_doc_batch, model="voyage-code-2", input_type="document",
                ).embeddings
        ]
        print("Embeddings preview: " + str(documents_embeddings[0][0][0:3]))
        print("")
```

The following is a more compact and less verbose code version: 

```python
import voyageai

vo = voyageai.Client()

# Assumes documents are in a list called `documents`

# Embed more than 128 documents
# you can use a for loop to embed them.
batch_size = 128

documents_embeddings = [
        vo.embed(
                documents[i:i + batch_size], model="voyage-code-2", input_type="document",
        ).embeddings
        for i in range(0, len(documents), batch_size)
]
```