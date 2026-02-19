100% locally hosted AI Recruitment Pipeline. It ingests PDF resumes, uses Agentic RAG (Retrieval-Augmented Generation) to analyze and score candidates, and builds a fully relational Knowledge Graph to easily search for candidate skills and contact information.

By running entirely offline using local LLMs, this architecture guarantees complete data privacy for sensitive HR documents.

🏗️ Architecture & Tech Stack
Workflow Orchestration: n8n (Self-Hosted)

Local AI Engine: Ollama

Embedding Model: nomic-embed-text (For Vectorizing Resumes)

Inference Model: eclipse (For Entity Extraction & Scoring)

Vector Database: ChromaDB (For semantic similarity search)

Graph Database: Neo4j (For relational entity mapping)

Custom Logic: JavaScript (For resilient JSON parsing and HR scoring logic)

✨ Key Features
Automated Candidate Scoring: The AI acts as a strict HR recruiter, grading candidates on a 100-point scale based on experience, title matching, and skill depth.

Resilient Data Extraction: Custom JavaScript error-handling ensures that even if the LLM hallucinates formatting, the pipeline recovers and formats the data correctly.

Dual-Database RAG:

ChromaDB stores the raw document embeddings for deep semantic searches.

Neo4j constructs a searchable graph of Candidates, Emails, Phone Numbers, Locations, and specific Skills.

Duplicate Prevention: Uses advanced Cypher MERGE queries to ensure skills and candidates are uniquely mapped without redundancy.

📸 System Previews
The Orchestration Pipeline (n8n)
[Upload a screenshot of your full n8n workflow here and replace this text]

The HR Knowledge Graph (Neo4j)
[Upload a screenshot of Fathur and Jovi connected to their skill bubbles here and replace this text]

🕸️ Knowledge Graph Schema
The Neo4j database automatically constructs the following relationships from the unstructured resumes:

(Person)-[:HAS_EMAIL]->(Email)

(Person)-[:HAS_PHONE]->(Phone)

(Person)-[:LIVED_AT]->(Location)

(Person)-[:HAS_SKILL]->(Skill)

🚀 How to Run This Project Locally
1. Prerequisites
You must have Docker and Docker Compose installed on your machine.

2. Start the Local AI Engine (Ollama)
Run the following commands in your terminal to start Ollama and download the required embedding model:

Bash
# Start Ollama on port 11434
docker run -d --name ollama -p 11434:11434 ollama/ollama

# Download the vector embedding model
docker exec -it ollama ollama pull nomic-embed-text

# (Optional) Pull the extraction model if not already installed
docker exec -it ollama ollama pull eclipse 
3. Setup Databases
Ensure your instances of ChromaDB and Neo4j are running locally.
(Note: If you used a docker-compose file for these, include that file in this repository!)

4. Import the n8n Workflow
Open your local n8n instance.

Go to Workflows -> Import from File.

Select the HR_RAG_Pipeline.json file included in this repository.

Update the Credentials in the Ollama, ChromaDB, and Neo4j nodes to match your local setup (e.g., using http://host.docker.internal:11434 for Windows Docker).

5. Run the Pipeline!
Upload a test PDF resume into the starting node and click Execute Workflow.
