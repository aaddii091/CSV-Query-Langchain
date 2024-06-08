# Project Title: Intelligent CSV Query Processor

## Overview
The Intelligent CSV Query Processor is a web-based application designed to provide users with the ability to upload CSV files and query their contents using natural language. Built with Vue.js and leveraging the Langchain framework, this application uses advanced natural language processing (NLP) techniques powered by OpenAI's GPT-4 to interpret and respond to user queries.

## Features
- **CSV File Upload**: Users can upload CSV files directly through the user interface.
- **Natural Language Querying**: Users can ask questions about the contents of their CSV files in plain English, and the application will provide relevant answers.
- **Real-time Processing**: The application processes the CSV file and returns query results in real-time, displaying a loading indicator while processing.
- **Embeddings and Vector Store**: The application uses OpenAI embeddings to convert document contents into vector representations, enabling efficient and accurate retrieval of information.
- **Text Splitting**: Large documents are split into manageable chunks to ensure effective processing and querying.
- **RetrievalQAChain**: Utilizes Langchain's RetrievalQAChain to handle the retrieval and querying of document contents.

## Technologies Used
- **Vue.js**: A progressive JavaScript framework for building user interfaces.
- **Langchain**: A framework for developing applications powered by language models.
- **OpenAI GPT-4**: A state-of-the-art language model used for natural language understanding and generation.
- **MemoryVectorStore**: An in-memory vector store for managing document embeddings.
- **RecursiveCharacterTextSplitter**: A utility to split text into smaller chunks for efficient processing.
- **CSVLoader**: A utility for loading and parsing CSV files.

## Detailed Workflow

### File Upload
The user uploads a CSV file through the application’s interface.

### File Processing
- The CSVLoader reads and parses the uploaded CSV file.
- Document contents are normalized and prepared for embedding.

### Text Splitting
The RecursiveCharacterTextSplitter splits the normalized document text into chunks of 1000 characters for easier handling.

### Embeddings Creation
- The split document chunks are converted into vector embeddings using OpenAIEmbeddings.
- These embeddings are stored in a MemoryVectorStore.

### Query Handling
- The user inputs a natural language query about the document (e.g., "Read the document and name top 3 horror movies").
- The ChatOpenAI model processes this query using the RetrievalQAChain.
- The application retrieves relevant information from the document and provides an answer.

## Setup and Usage

### Environment Setup
1. Ensure you have Node.js and npm installed.
2. Create a `.env` file in the project root and add your OpenAI API key:
    ```plaintext
    VUE_APP_OPENAI_API_KEY=your_openai_api_key
    ```

### Install Dependencies
```sh
npm install
