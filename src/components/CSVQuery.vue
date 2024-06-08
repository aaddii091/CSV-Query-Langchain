<template>
  <div>
    <h1>Upload CSV File</h1>
    <input type="file" @change="handleFileUpload" accept=".csv" />
    <div v-if="loading">Processing...</div>
    <div v-if="result">
      <h2>Query Result</h2>
      <pre>{{ result }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { ChatOpenAI } from "@langchain/openai";
import { RetrievalQAChain } from "langchain/chains";
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { OpenAIEmbeddings } from "@langchain/openai";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { CSVLoader } from "langchain/document_loaders/fs/csv";

// Define state variables
const loading = ref(false);
const result = ref(null);

// Handle file upload
const handleFileUpload = async (event) => {
  const file = event.target.files[0];
  if (file && file.name.endsWith(".csv")) {
    loading.value = true;

    try {
      // // Convert file to text
      // const text = await file.text();
      // console.log(text);
      // Process CSV
      const loader = new CSVLoader(file);
      const docs = await loader.load();
      console.log(docs);

      // Normalize documents
      const normalizedDocs = docs.map((doc) => doc.pageContent);

      // Create vector store
      const textSplitter = new RecursiveCharacterTextSplitter({
        chunkSize: 1000,
      });
      const splitDocs = await textSplitter.createDocuments(normalizedDocs);

      const vectorStore = await MemoryVectorStore.fromDocuments(
        splitDocs,
        new OpenAIEmbeddings({
          apiKey: import.meta.env.VITE_OPENAI_API_KEY,
        })
      );

      // Initialize OpenAI model and create retrieval chain
      const model = new ChatOpenAI({
        apiKey: import.meta.env.VITE_OPENAI_API_KEY,
      });
      const chain = RetrievalQAChain.fromLLM(model, vectorStore.asRetriever());

      // Query the retrieval chain
      const question = "read the document and name top 3 horror movies";
      const res = await chain.invoke({ query: question });
      result.value = res.text;
    } catch (error) {
      console.error("Error processing the file:", error);
    } finally {
      loading.value = false;
    }
  } else {
    alert("Please upload a valid CSV file.");
  }
};
</script>
