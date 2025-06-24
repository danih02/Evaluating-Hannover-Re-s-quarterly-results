# 📊 Q&A Agent: Hannover Re's Quarterly Results (Q1 2025)

This project demonstrates a simple LangGraph-based agent designed to answer questions related to **Hannover Re's Q1 2025 quarterly results**. It utilizes Google Gemini language models to extract and structure financial insights from the official earnings presentation.

---

## 🔧 Overview

The system uses **two LLMs** inside a LangGraph workflow:

- **`gemini-2.5-flash`**: Extracts answers directly from the provided financial documents.
- **`gemini-2.0-flash`**: Reformats the extracted answers into a clean, structured summary.

---

## 🧠 Architecture

### 1. Document Loader

The agent loads the Q1 2025 earnings presentation from:

📄 [ConCall_May2025.pdf](https://assets.hannover-re.com/asset/533267266226/document_ccm2p880911ul0kum1pracq67k/ConCall_May2025.pdf?content-disposition=inline)

```python
from langchain.document_loaders import PyPDFLoader

loader = PyPDFLoader("ConCall_May2025.pdf")
documents = loader.load()
```

---

### 2. LangGraph Workflow

#### Nodes

- **Answering Node (`gemini-2.5-flash`)**: Extracts short and precise answers.
- **Summarizing Node (`gemini-2.0-flash`)**: Structures multiple answers into a formatted output.

#### Graph Flow

```
START → [Answering Node] → [Summarizing Node] → FINAL
```

The LangGraph uses `StateGraph` to define and connect these nodes.

---

### 3. Prompt Templates

- **Analyst Prompt**: Focused, grounded in the document, no hallucinations.
- **Summarization Prompt**: Outputs exactly N structured responses for N questions.

---

### 4. Evaluation

Example questions cover:
- **General financial performance**
- **Life & Health business**
- **Property & Casualty business**

Answers are validated against ground truth and are **all correct**.

---

## 📥 Installation

```bash
pip install langchain-community langchain_google_genai langgraph
```

---

## 🔐 API Setup

Set up your API key:

```bash
export GOOGLE_API_KEY="your-google-api-key"
```

Or in Python:

```python
import os
os.environ["GOOGLE_API_KEY"] = "your-google-api-key"
```

---

## 🚀 Usage

Run the pipeline with:

```python
graph.invoke({"input": list_of_questions})["final"].list_answers
```

This returns a structured list of concise answers.

---

## ✅ Future Enhancements

- Integrate **retrieval tools** (e.g., DuckDuckGo, Tavily) to locate presentations automatically.
- Add **PDF semantic parsing** for charts and images.
- Introduce **reranking or verification** models to handle ambiguous questions.
