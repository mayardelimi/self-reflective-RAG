# NexaTech Self-Reflective RAG System

The NexaTech Self-Reflective RAG System is a fault-tolerant, production-grade pipeline that uses LangGraph to eliminate hallucinations and ensure reliable answers from corporate documents through multi-step validation loops and strict grounding verification.

---

## 1. Limitations of Classic RAG

Traditional RAG pipelines follow a rigid sequence that causes several architectural bottlenecks:
* **Blind Retrieval Overhead:** Wastes resources by forcing retrieval even for general or conversational queries.
* **Context Pollution:** Standard vector similarity search frequently retrieves semantically related but unhelpful chunks, degrading answer quality.
* **No Grounding Verification:** Lacks the ability to verify if the LLM's response is strictly supported by the context, allowing for fabricated information.
* **Lack of Self-Correction:** Fails to provide a feedback loop to revise or refine partially supported or inaccurate answers.

---

## 2. Self-Reflective RAG Features

This project utilizes a LangGraph-managed workflow to overcome classic limitations with the following capabilities:
* **Dynamic Retrieval:** Evaluates queries and bypasses vector retrieval for general explanations to optimize performance.
* **Relevance Filtering:** Inspects and discards irrelevant chunks before generation to prevent context pollution.
* **Direct & Grounded Generation:** Securely handles general queries directly, while synthesizing verified business answers solely from relevant document chunks.
* **Strict Grounding Verification (`is_sup`):** Evaluates answers against the context across multiple support levels, strictly disallowing unauthorized interpretations or unsupported adjectives.
* **Iterative Self-Correction (`revise_answer`):** Triggers automatic revision loops for unsupported answers to enforce strict quote-only extraction.
* **Usefulness Judgment (`is_use`):** Evaluates the final response and routes to fallback handlers if the answer fails to address the user's intent.

---

## 3. Technology Stack

* **Orchestration:** LangGraph & LangChain Core
* **LLM & Inference:** ChatGroq (`openai/gpt-oss-120b`)
* **Embeddings:** `BAAI/bge-small-en-v1.5` (Local)
* **Vector Store:** FAISS
* **Structured Outputs:** Pydantic
* **Document Parsing:** LangChain Community PDF Loaders & `RecursiveCharacterTextSplitter`

---

## 4. Workflow Graph

The diagram below illustrates the complete state machine and conditional routing logic executed during a user query cycle:

![Self-Reflective RAG State Graph](docs/plots/image.png)
