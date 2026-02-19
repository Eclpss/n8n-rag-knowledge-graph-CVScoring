# 🧠 Local AI HR Agent: Graph-Based Resume Parsing & RAG Pipeline

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen)

## 📌 Project Overview
This project is an automated, **100% locally hosted AI Recruitment Pipeline**. It ingests PDF resumes, uses Agentic RAG (Retrieval-Augmented Generation) to analyze and score candidates, and builds a fully relational **Knowledge Graph** to easily search for candidate skills and contact information.

By running entirely offline using local LLMs, this architecture guarantees complete data privacy for sensitive HR documents.

## 🏗️ Architecture & Tech Stack
* **Workflow Orchestration:** [n8n](https://n8n.io/) (Self-Hosted)
* **Local AI Engine:** [Ollama](https://ollama.ai/)
* **Embedding Model:** `nomic-embed-text` (For Vectorizing Resumes)
* **Inference Model:** `eclipse` (For Entity Extraction & Scoring)
* **Vector Database:** [ChromaDB](https://www.trychroma.com/) (For semantic similarity search)
* **Graph Database:** [Neo4j](https://neo4j.com/) (For relational entity mapping)
* **Custom Logic:** JavaScript (For resilient JSON parsing and HR scoring logic)

## ✨ Key Features
1. **Automated Candidate Scoring:** The AI acts as a strict HR recruiter, grading candidates on a 100-point scale based on experience, title matching, and skill depth.
2. **Resilient Data Extraction:** Custom JavaScript error-handling ensures that even if the LLM hallucinates formatting, the pipeline recovers and formats the data correctly.
3. **Dual-Database RAG:**
   * **ChromaDB** stores the raw document embeddings for deep semantic searches.
   * **Neo4j** constructs a searchable graph of Candidates, Emails, Phone Numbers, Locations, and specific Skills.
4. **Duplicate Prevention:** Uses advanced Cypher `MERGE` queries to ensure skills and candidates are uniquely mapped without redundancy.

---

## 📸 System Previews

### The Orchestration Pipeline (n8n) there are some gemini embbeddings to do so jst use the api key from the gemini website
<img width="989" height="485" alt="image" src="https://github.com/user-attachments/assets/68c249c1-52cf-413c-9469-444942109cea" />


### The HR Knowledge Graph (Neo4j) example
<img width="1039" height="444" alt="image" src="https://github.com/user-attachments/assets/f3bb90d0-008e-40d8-9f83-f5f79255c0b9" />


---

## 🕸️ Knowledge Graph Schema
The Neo4j database automatically constructs the following relationships from the unstructured resumes:
* `(Person)-[:HAS_EMAIL]->(Email)`
* `(Person)-[:HAS_PHONE]->(Phone)`
* `(Person)-[:LIVED_AT]->(Location)`
* `(Person)-[:HAS_SKILL]->(Skill)`

---

## 🚀 How to Run This Project Locally

### 1. Prerequisites
You must have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running on your machine.

### 2. Start the Databases (Neo4j & ChromaDB)
Run these commands in your terminal to instantly spin up the Graph and Vector databases:

**Start Neo4j (Graph Database):**
```bash
docker run -d --name neo4j -p 7474:7474 -p 7687:7687 -e NEO4J_AUTH=neo4j/password neo4j:latest
