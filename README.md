# Medical RAG Assistant

A retrieval-augmented generation project that answers clinical questions using grounded medical reference content from the Merck Manual.

## Project Overview

Healthcare professionals often need fast, reliable answers while working under time pressure. However, medical knowledge is spread across long, complex manuals, making it difficult to quickly retrieve the right information when it matters most.

This project explores how a Large Language Model (LLM) can be improved with Retrieval-Augmented Generation (RAG) to provide more reliable, source-grounded answers to clinical questions.

The project compares three approaches:
- LLM-only question answering
- LLM with prompt engineering
- RAG-based question answering grounded in Merck Manual content

## Business Problem

Medical decision-making is high stakes. Delays, incomplete information, and hallucinated answers can lead to inconsistent care or diagnostic errors.

This project addresses a core challenge in healthcare AI:
- clinicians need fast access to trusted medical knowledge
- answers should be traceable to an authoritative source
- systems should reduce hallucination risk
- AI should support, not replace, clinical judgment

## Solution Approach

The solution uses a RAG pipeline to ground answers in a trusted medical reference source.

High-level workflow:
- PDF medical reference manual
- text chunking
- embeddings generation
- vector database indexing
- semantic retrieval
- LLM answer generation using retrieved context

The development progression for the project was:
1. LLM-only baseline
2. Prompt-engineered LLM
3. RAG-based medical assistant

## Dataset

The knowledge source for this project was the **Merck Medical Manual** in PDF format.

Dataset characteristics:
- approximately 4,000+ pages
- structured medical reference content
- covers symptoms, diagnosis, treatment protocols, procedures, emergency care guidance, and disease classifications
- organized across multiple medical specialties and body systems

This dataset was selected because it is a trusted, medically reviewed resource that is well suited for document chunking and retrieval.

## LLM Baseline

The base model used for question answering was:

- **Model:** Mistral-7B-Instruct-v0.2 (GGUF)
- loaded through llama-cpp
- selected for instruction-following behavior, efficient inference, and compatibility with RAG workflows

The LLM-only version generated coherent and medically structured answers, but responses were not grounded in the Merck Manual and lacked citation traceability.

## Prompt Engineering

Prompt engineering improved response structure and consistency without adding retrieval.

### System Prompt

You are a clinical medical assistant. Provide structured, step-by-step, medically accurate responses. Avoid speculation. Clearly distinguish symptoms, diagnosis, and treatment. If uncertain, state limitations explicitly.

### Best Parameters

- **Temperature:** 0.2
- **Max Tokens:** 350
- **Top-p:** 0.9
- **Top-k:** 40

### Observations

Prompt engineering improved:
- structure and organization
- step-by-step clarity
- consistency across clinical queries
- appropriate uncertainty handling

However, answers were still not grounded in an external source, so hallucination risk remained.

## Data Preparation for RAG

The Merck Manual PDF was converted into retrievable chunks for semantic search.

### Text Splitting Configuration

- **Splitter:** RecursiveCharacterTextSplitter
- **Chunk Size:** 1000 characters
- **Chunk Overlap:** 200 characters
- **Total Chunks Created:** 4685

This configuration helped preserve contextual continuity while improving retrieval quality.

## Embeddings and Vector Database

### Embedding Model

- **SentenceTransformer:** all-MiniLM-L6-v2

Why it was used:
- lightweight and efficient
- strong semantic similarity performance
- suitable for large-scale indexing

### Vector Database

- **Chroma DB**
- persisted locally as `medical_db`

## RAG Configuration

### Retriever Settings

- **Retriever Type:** similarity search
- **k:** 3

### Generation Settings

- **Model:** Mistral-7B-Instruct-v0.2
- **Temperature:** 0.2
- **Max Tokens:** 350
- **Top-p:** 0.9
- **Top-k:** 40

### RAG Prompt Structure

**System Prompt**
Use only the provided context from the Merck Manual. If the information is not available in the context, clearly state that it is not found.

**User Prompt Template**
- Context: retrieved chunks
- Question: user query
- Output: structured medical answer based strictly on the provided context

### Why This Configuration Was Selected

The final configuration provided the best balance of:
- completeness
- focus
- reduced redundancy
- lower hallucination risk
- clinically consistent phrasing

## Evaluation

The project used an LLM-as-judge framework to evaluate answer quality across two dimensions:

### Groundedness
Measures whether the answer is supported by the retrieved context.

### Relevance
Measures how directly and completely the answer addresses the question.

### Evaluation Takeaways

- RAG improved groundedness compared to LLM-only responses
- relevance remained consistently strong across queries
- retrieval quality had a direct impact on answer reliability
- partial grounding occurred when retrieved context lacked enough procedural depth

## Key Findings

- LLM-only responses were coherent but unsourced
- prompt engineering improved structure and consistency
- RAG produced more reliable, better-grounded clinical answers
- retrieval quality and chunking strategy strongly influenced system performance
- domain grounding improved traceability and reduced hallucination risk

## Business Impact

This project demonstrates how RAG can make AI assistants more useful in high-stakes, information-dense environments such as healthcare.

Potential value includes:
- faster access to trusted medical knowledge
- improved factual grounding
- reduced hallucination risk
- better traceability to source material
- support for consistent clinical decision support workflows

## Recommendations

Recommended next steps for improving and extending the system:

- improve retrieval tuning and chunking strategy
- test alternative embedding models
- expand the system to internal hospital knowledge bases and clinical protocols
- add stronger monitoring for groundedness and relevance
- maintain human oversight in any real clinical setting
- position the system as decision support, not autonomous diagnosis

## Tools and Techniques

- Python
- Natural Language Processing (NLP)
- Large Language Models (LLMs)
- Retrieval-Augmented Generation (RAG)
- Prompt Engineering
- Mistral-7B-Instruct
- Sentence Transformers
- Chroma DB
- Semantic Search
- LLM-as-judge Evaluation

## Repository Contents

This repository is intended to showcase the project workflow, modeling approach, RAG architecture, and evaluation results.

Suggested files for this repo:
- `README.md`
- `medical_rag_assistant_notebook.ipynb`
- `Medical_RAG_Assistant_Presentation.pdf`

## Important Note

This project is for educational and portfolio purposes only. It is not intended for real-world clinical diagnosis or treatment without qualified medical oversight.

## Key Takeaway

Grounding a medical question-answering system in trusted reference content significantly improves answer reliability. This project shows how RAG can enhance traceability, reduce hallucination risk, and create safer decision-support workflows in healthcare AI.
