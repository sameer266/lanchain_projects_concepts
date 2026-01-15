

````markdown
# 📘 PDF Q&A Bot (DeepSeek + LangChain + FAISS)

This project allows you to **ask questions about PDF documents** using a local LLM (DeepSeek) with **RAG (Retrieval-Augmented Generation)**. The bot reads your PDFs, finds relevant text, and answers your questions accurately.

---

## 🧩 Features

- Load **single or multiple PDFs** from a folder
- Split PDFs into **small chunks** for better processing
- Convert text chunks into **vectors** (numeric representation of meaning)
- Store vectors in **FAISS vector database** for fast search
- Use **DeepSeek LLM** to generate **human-readable answers**
- Command-line interface (CLI) for interactive Q&A

---

## 🏗️ How It Works (Flow)

```text
[PDF(s)]
   ↓  (split into chunks)
[Text chunks]
   ↓  (convert to vectors)
[Vector Database (FAISS)]
   ↓  (search relevant chunks)
[Relevant chunks]
   ↓  (DeepSeek LLM)
[Answer to User]
````

**Step-by-step:**

1. **Load PDF(s)** → Read all PDF text into Python.
2. **Split into chunks** → Each chunk is small enough for the LLM to process.
3. **Convert to vectors** → Turn text into numbers that represent meaning.
4. **Store in vector DB** → FAISS keeps vectors for **fast similarity search**.
5. **Ask a question** → Vector DB finds the **most relevant chunks**.
6. **DeepSeek LLM generates answer** → Produces human-readable response.

---

## ⚙️ Installation

1. Create and activate a virtual environment (Python 3.10 or 3.11 recommended):

```bash
python -m venv venv
venv\Scripts\activate      # Windows
# source venv/bin/activate # Linux / Mac
```

2. Install required packages:

```bash
pip install langchain langchain_community langchain-core
pip install pypdf faiss-cpu
```

---

## 🧑‍💻 Project Structure

```
pdf_qa_bot/
│
├── pdf_qa.py           # Main script
├── pdfs/               # Folder for PDF files
│   ├── sample1.pdf
│   └── sample2.pdf
└── README.md
```

---

## 💻 Code (`pdf_qa.py`)

```python
from langchain_community.llms import Ollama
from langchain_community.document_loaders import PyPDFLoader
from langchain_community.embeddings import OllamaEmbeddings
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import FAISS
from langchain.chains import RetrievalQA
import os

# 1️⃣ Load all PDFs from folder
pdf_folder = "pdfs"
documents = []
for file in os.listdir(pdf_folder):
    if file.endswith(".pdf"):
        loader = PyPDFLoader(os.path.join(pdf_folder, file))
        documents.extend(loader.load())

# 2️⃣ Split text into chunks
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,   # each chunk = 500 characters
    chunk_overlap=100 # repeat 100 characters for context
)
chunks = text_splitter.split_documents(documents)

# 3️⃣ Create embeddings
embeddings = OllamaEmbeddings(model="deepseek-r1:1.5b")

# 4️⃣ Create FAISS vector database
vectorstore = FAISS.from_documents(chunks, embeddings)

# 5️⃣ Load DeepSeek LLM
llm = Ollama(
    model="deepseek-r1:1.5b",
    temperature=0.2
)

# 6️⃣ Create RetrievalQA chain
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever()
)

# 7️⃣ CLI loop
print("📘 PDF Q&A Bot (type 'exit' to quit)\n")
while True:
    question = input("You: ")
    if question.lower() == "exit":
        print("👋 Goodbye")
        break
    answer = qa_chain.run(question)
    print("Bot:", answer)
```

---

## 🧠 Example Usage

```
You: What are Django signals?
Bot: Django signals let parts of your app respond automatically to events like saving or deleting data.

You: Summarize chapter 2
Bot: Chapter 2 explains models in Django, which define database tables and relationships.
```

---

## 🔹 Notes

* `chunk_size` → How big each piece of text is (500 characters recommended)
* `chunk_overlap` → Keeps 100 characters repeated for context
* Vector DB = **search tool** for relevant chunks
* LLM (DeepSeek) = **brain that generates human-readable answers**

---

## 🚀 Next Improvements

* Save FAISS database to disk → **load without reprocessing PDFs**
* Add memory → remember previous questions
* Add a web interface → Flask / Django app
* Multi-language PDFs → support non-English text

---

## 🎯 Summary

This project demonstrates **RAG (Retrieval-Augmented Generation)** using PDFs and a **local LLM (DeepSeek)**.
It ensures **accurate answers from your own documents** while working offline.

```

---
