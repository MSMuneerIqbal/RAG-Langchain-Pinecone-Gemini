
# RAG-Langchain-Pinecone

This project demonstrates a Retrieval-Augmented Generation (RAG) pipeline using LangChain, Pinecone, and Google's Generative AI. It processes documents, generates embeddings, stores them in a Pinecone index, and allows question answering through a retrieval-based query system integrated with a generative AI model.

## Features

- **Document Processing**: Reads and processes PDF documents using `PyPDFLoader`.
- **Text Chunking**: Splits documents into smaller chunks for embedding and indexing.
- **Embeddings**: Generates embeddings using Google Generative AI's embedding model.
- **Pinecone Integration**: Stores and retrieves document embeddings from a Pinecone vector database.
- **Question Answering**: Implements a RAG pipeline for answering user queries based on indexed documents.
- **Generative AI Integration**: Uses Google's Gemini model for natural language understanding and response generation.

## Installation

Ensure you have Python installed. Then, install the required dependencies:

```bash
pip install -qU python-dotenv langchain pinecone-client google-generativeai tqdm langchain-community pypdf
```

## Environment Variables

Set the following environment variables to enable API integrations:

- `PINECONE_API_KEY`: Your Pinecone API key.
- `PINECONE_ENVIRONMENT_KEY`: Your Pinecone environment key.
- `GOOGLE_API_KEY`: Your Google Generative AI API key.

## Usage

1. **Document Loading**: Replace `/content/Effective Software.pdf` with the path to your own document.
   ```python
   loader = PyPDFLoader("/path/to/your/document.pdf")
   documents = loader.load()
   ```

2. **Index Connection**:
   ```python
   index_name = 'your-index-name'
   index = pc.Index(index_name)
   ```

3. **Run the Script**:
   - The script splits the document into chunks, generates embeddings, and uploads them to Pinecone.
   - It then allows querying the stored data using the RAG pipeline.

4. **Query Examples**:
   ```python
   query = "give details Effective Software Engineer"
   response = qa_chain.run(query)
   print(response)
   ```

## Output

- The script processes the document and outputs key details, such as:
  - Total characters in the document.
  - Number of chunks created.
  - Responses to user queries.

## Dependencies

- `python-dotenv`: For managing environment variables.
- `langchain`: Core framework for building language model applications.
- `pinecone-client`: To interface with Pinecone's vector database.
- `google-generativeai`: To leverage Google Generative AI models.
- `tqdm`: For progress bars.
- `pypdf`: For handling PDF documents.

## Key Functions

- **Document Embedding and Upload**:
   ```python
   for i, doc in enumerate(tqdm(docs)):
       vector = embeddings.embed_query(doc.page_content)
       index.upsert(vectors=[(str(i), vector, {"text": doc.page_content})])
   ```

- **Query Answering**:
   ```python
   retriever = VectorStoreRetriever(vectorstore=Pinecone.from_existing_index(index_name=index_name, embedding=embeddings, text_key="text"))
   qa_chain = RetrievalQA.from_chain_type(llm=gemini_model, chain_type="map_reduce", retriever=retriever)
   response = qa_chain.run("Your Query")
   ```

## Notes

- Make sure your API keys are valid and have sufficient quotas.
- Customize the document path and index name as needed.

## References

- [LangChain Documentation](https://langchain.readthedocs.io/)
- [Pinecone Documentation](https://docs.pinecone.io/)
- [Google Generative AI](https://cloud.google.com/ai-platform/)

---

Feel free to contribute or raise issues for improvements!

