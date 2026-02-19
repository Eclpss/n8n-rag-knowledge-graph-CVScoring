🚀 How to Run This Project Locally
1. Prerequisites
You must have Docker Desktop installed and running on your machine.

2. Start the Databases (Neo4j & ChromaDB)
Run these commands in your terminal to instantly spin up the Graph and Vector databases:

Start Neo4j (Graph Database):

Bash
docker run -d --name neo4j -p 7474:7474 -p 7687:7687 -e NEO4J_AUTH=neo4j/password neo4j:latest
(Access the Neo4j browser at http://localhost:7474 using username: neo4j and password: password)

Start ChromaDB (Vector Database):

Bash
docker run -d --name chromadb -p 8000:8000 chromadb/chroma
3. Start the Local AI Engine (Ollama)
Run the following commands to start Ollama and download the required embedding model:

Bash
# Start Ollama on port 11434
docker run -d --name ollama -p 11434:11434 ollama/ollama

# Download the vector embedding model
docker exec -it ollama ollama pull nomic-embed-text

# (Optional) Pull the extraction model if you don't have it installed
docker exec -it ollama ollama pull eclipse 
4. Start n8n (Orchestration Engine)
Run n8n locally using this command:

Bash
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
(Access your n8n workspace at http://localhost:5678)

5. Import the Workflow
Open your local n8n instance at http://localhost:5678.

Go to Workflows -> Import from File.

Select the HR_RAG_Pipeline.json file included in this repository.

Update the Credentials in the Ollama, ChromaDB, and Neo4j nodes to connect to your local Docker containers (e.g., use http://host.docker.internal:11434 for Ollama on Windows).

6. Run the Pipeline!
Upload a test PDF resume into the starting node and click Execute Workflow to watch the Knowledge Graph build itself.
