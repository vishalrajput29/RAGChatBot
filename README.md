# Project Report: Conversational RAG with PDF Uploads and Chat History

## **1. Project Overview**
The project is a Streamlit-based application that implements a Retrieval-Augmented Generation (RAG) system for conversational question-answering using uploaded PDF documents. It allows users to upload PDF files, interact with their content through natural language queries, and maintain chat history for contextualized conversations. The application leverages advanced AI models like Groq's `Gemma2-9b-It` and HuggingFace embeddings (`all-MiniLM-L6-v2`) for efficient document processing and response generation.

---

## **2. Key Features**

### **2.1 PDF Document Processing**
- **PDF Upload**: Users can upload one or more PDF files via the Streamlit interface.
- **Document Splitting**: Uploaded PDFs are split into manageable chunks using `RecursiveCharacterTextSplitter` with a chunk size of 5000 and overlap of 500.
- **Embedding Creation**: The text chunks are embedded using HuggingFace's `all-MiniLM-L6-v2` model for semantic search.

### **2.2 Vector Store and Retrieval**
- **FAISS Vector Store**: The embedded chunks are stored in a FAISS vector store for efficient similarity-based retrieval.
- **Retriever**: A retriever is created from the vector store to fetch relevant document chunks based on user queries.

### **2.3 Contextualized Question Handling**
- **Chat History Awareness**: The system reformulates user queries by considering the chat history to ensure context-aware responses.
- **Standalone Query Formation**: If a query references past interactions, it is reformulated into a standalone question for better understanding.

### **2.4 Conversational RAG Chain**
- **RAG Pipeline**: Combines retrieval and generation to provide accurate answers based on the retrieved context.
- **Chat History Management**: Maintains session-specific chat history using `ChatMessageHistory` and integrates it into the RAG pipeline for contextualized responses.

### **2.5 User Interface**
- **Streamlit App**: Provides an intuitive interface for uploading PDFs, entering API keys, and interacting with the system.
- **Session Management**: Allows users to define session IDs for stateful chat history management.
- **Real-Time Responses**: Displays assistant responses and chat history dynamically.

---

## **3. Common Problems Faced During Development**

### **3.1 Integration Challenges**
- **API Key Dependency**: The application requires a valid Groq API key for the LLM (`Gemma2-9b-It`). Missing or invalid keys lead to runtime errors.
- **Environment Variables**: Managing sensitive tokens (e.g., HF_TOKEN) securely required careful handling with `.env` files.

### **3.2 Document Processing**
- **Large PDF Files**: Processing large PDFs caused memory issues due to extensive splitting and embedding. This was mitigated by optimizing chunk sizes and overlaps.
- **Text Extraction Errors**: Some PDFs with complex layouts (e.g., scanned images, non-standard fonts) led to incomplete or incorrect text extraction. Preprocessing steps were added to handle such cases.

### **3.3 Contextualization**
- **Ambiguous Queries**: Reformulating user queries based on chat history was challenging when queries lacked sufficient context. Fine-tuning the `contextualize_q_prompt` improved performance.
- **Overfitting to History**: Excessive reliance on chat history sometimes resulted in irrelevant responses. Balancing context usage was crucial.

### **3.4 Performance Optimization**
- **Latency**: Embedding creation and similarity searches introduced delays. Using FAISS optimized retrieval speed but required significant computational resources.
- **Scalability**: Handling multiple sessions simultaneously increased memory usage. Efficient session management and cleanup mechanisms were implemented.

### **3.5 Debugging and Testing**
- **Error Handling**: Identifying and resolving runtime errors (e.g., missing dependencies, incorrect configurations) required extensive debugging.
- **Edge Cases**: Testing with diverse PDFs and queries revealed edge cases, such as empty documents or malformed inputs, which were addressed with robust validation checks.

---

## **4. Technologies and Libraries Used**

| **Technology/Library**       | **Purpose**                                                                 |
|-------------------------------|-----------------------------------------------------------------------------|
| **Streamlit**                 | Building the interactive web interface.                                     |
| **LangChain**                 | Implementing the RAG pipeline, including retrieval and generation chains.   |
| **FAISS**                     | Efficient similarity search over embedded document chunks.                  |
| **HuggingFace Embeddings**    | Generating semantic embeddings for text chunks.                             |
| **PyPDFLoader**               | Extracting text from uploaded PDF files.                                    |
| **Groq API**                  | Powering the LLM (`Gemma2-9b-It`) for generating responses.                 |
| **dotenv**                    | Managing environment variables securely.                                   |

---

## **5. Future Enhancements**

### **5.1 Improved Document Handling**
- Support for additional file formats (e.g., DOCX, TXT).
- Enhanced text extraction techniques for scanned or image-based PDFs using OCR tools like Tesseract.

### **5.2 Advanced Query Understanding**
- Incorporate Named Entity Recognition (NER) to identify and prioritize key entities in queries.
- Use multi-turn dialogue models for better contextual understanding.

### **5.3 Scalability**
- Deploy the application on cloud platforms (e.g., AWS, GCP) for scalability and high availability.
- Optimize resource usage for handling large-scale deployments.

### **5.4 User Experience**
- Add features like progress indicators for document processing and response generation.
- Provide downloadable chat transcripts for user convenience.

### **5.5 Security**
- Encrypt sensitive data (e.g., API keys, session histories) to enhance security.
- Implement user authentication for personalized sessions.

---

## **6. Conclusion**
The Conversational RAG application successfully combines advanced NLP techniques with a user-friendly interface to enable seamless interaction with PDF content. While challenges related to integration, performance, and contextualization were encountered, they were effectively addressed through iterative development and optimization. The project lays a strong foundation for building scalable, intelligent systems capable of handling complex conversational tasks.

---

## **7. Appendix**

### **7.1 Code Structure**
- **Main Script**: `app.py` contains the core logic for document processing, RAG pipeline implementation, and Streamlit UI.
- **Dependencies**: Managed via `requirements.txt` (not shown in the code snippet).

### **7.2 Deployment Instructions**
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Set up environment variables in `.env`:
   ```env
   HF_TOKEN=your_huggingface_token
   ```
3. Run the app:
   ```bash
   streamlit run app.py
   ```

### **7.3 Example Workflow**
1. Upload a PDF file.
2. Enter a Groq API key.
3. Ask questions about the PDF content.
4. View responses and chat history in real-time.

