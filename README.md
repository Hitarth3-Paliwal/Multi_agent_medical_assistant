# Overview
The Multi-Agent Medical Assistant is an AI-powered chatbot designed to assist with medical diagnosis, research, and patient interactions.

## 🔍 Powered by Multi-Agent Intelligence

This system integrates the following technologies:

-  **Large Language Models (LLMs)** for natural language understanding and generation  
-  **Computer Vision Models** for medical imaging analysis and diagnosis  
-  **Retrieval-Augmented Generation (RAG)** using vector databases to enhance context relevance  
-  **Real-time Web Search** to provide up-to-date medical insights  
-  **Human-in-the-Loop Validation** to ensure accuracy in AI-based medical image diagnoses  

🤖 **Multi-Agent Architecture** : Specialized agents working in harmony to handle diagnosis, information retrieval, reasoning, and more

- 🔍 **Advanced Agentic RAG Retrieval System** :

  - Docling based parsing to extract text, tables, and images from PDFs.
  - Embedding markdown formatted text, tables and LLM based image summaries.
  - LLM based semantic chunking with structural boundary awareness.
  - LLM based query expansion with related medical domain terms.
  - Qdrant hybrid search combining BM25 sparse keyword search along with dense embedding vector search.
  - HuggingFace Cross-Encoder based reranking of retrieved document chunks for accurate LLM reponses.
  - Input-output guardrails to ensure safe and relevant responses.
  - Links to source documents and images present in reference document chunks provided with reponse.
  - Confidence-based agent-to-agent handoff between RAG and Web Search to prevent hallucinations.

- 🏥 **Medical Imaging Analysis**  
  - Brain Tumor Detection (TBD)
  - Chest X-ray Disease Classification
  - Skin Lesion Segmentation

- 🌐 **Real-time Research Integration** : Web search agent that retrieves the latest medical research papers and findings

- 📊 **Confidence-Based Verification** : Log probability analysis ensures high accuracy in medical recommendations

- 🎙️ **Voice Interaction Capabilities** : Seamless speech-to-text and text-to-speech powered by Eleven Labs API

- 👩‍⚕️ **Expert Oversight System** : Human-in-the-loop verification by medical professionals before finalizing outputs

- ⚔️ **Input & Output Guardrails** : Ensures safe, unbiased, and reliable medical responses while filtering out harmful or misleading content

- 💻 **Intuitive User Interface** : Designed for healthcare professionals with minimal technical expertise



## 💫 Demo <a name="demo"></a>


https://github.com/user-attachments/assets/d27d4a2e-1c7d-45e2-bbc5-b3d95ccd5b35


**For an even more detailed demo video**: Check out [`Multi-Agent-Medical-Assistant-v1.9`](assets/Multi-Agent-Medical-Assistant-v1.9_Compressed.mp4). 📽


 ## 🚀 Installation & Setup  <a name="installation-setup"></a>

## 📌 Option 1: Using Docker  <a name="docker-setup"></a>

### Prerequisites:

- [Docker](https://docs.docker.com/get-docker/) installed on your system
- API keys for the required services

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/souvikmajumder26/Multi-Agent-Medical-Assistant.git
cd Multi-Agent-Medical-Assistant
```

### 2️⃣ Create Environment File
- Create a `.env` file in the root directory and add the following API keys:

> [!NOTE]  
> You may use any llm and embedding model of your choice...
> 1. If using Azure OpenAI, no modification required.
> 2. If using direct OpenAI, modify the llm and embedding model definitions in the 'config.py' and provide appropriate env variables.
> 3. If using local models, appropriate code changes might be required throughout the codebase especially in 'agents'.

> [!WARNING]  
> Ensure the API keys in the `.env` file are correct and have the necessary permissions.
> No trailing whitespaces after variable names.

```bash
# LLM Configuration (Azure Open AI - gpt-4o used in development)
# If using any other LLM API key or local LLM, appropriate code modification is required
deployment_name= 
model_name=gpt-4o
azure_endpoint=
openai_api_key=
openai_api_version=

# Embedding Model Configuration (Azure Open AI - text-embedding-ada-002 used in development)
# If using any other embedding model, appropriate code modification is required
embedding_deployment_name=
embedding_model_name=text-embedding-ada-002
embedding_azure_endpoint=
embedding_openai_api_key=
embedding_openai_api_version=

# Speech API Key (Free credits available with new Eleven Labs Account)
ELEVEN_LABS_API_KEY=

# Web Search API Key (Free credits available with new Tavily Account)
TAVILY_API_KEY=

# Hugging Face Token - using reranker model "ms-marco-TinyBERT-L-6"
HUGGINGFACE_TOKEN=

# (OPTIONAL) If using Qdrant server version, local does not require API key
QDRANT_URL=
QDRANT_API_KEY=
```

### 3️⃣ Build the Docker Image
```bash
docker build -t medical-assistant .
```

### 4️⃣ Run the Docker Container
```bash
docker run -d --name medical-assistant-app -p 8000:8000 --env-file .env medical-assistant
```
The application will be available at: [http://localhost:8000](http://localhost:8000)

### 5️⃣ Ingest Data into Vector DB from Docker Container

- To ingest a single document:
```bash
docker exec medical-assistant-app python ingest_rag_data.py --file ./data/raw/brain_tumors_ucni.pdf
```

- To ingest multiple documents from a directory:
```bash
docker exec medical-assistant-app python ingest_rag_data.py --dir ./data/raw
```

### Managing the Container:

#### Stop the Container
```bash
docker stop medical-assistant-app
```

#### Start the Container
```bash
docker start medical-assistant-app
```

#### View Logs
```bash
docker logs medical-assistant-app
```

#### Remove the Container
```bash
docker rm medical-assistant-app
```

### Troubleshooting:

#### Container Health Check
The container includes a health check that monitors the application status. You can check the health status with:
```bash
docker inspect --format='{{.State.Health.Status}}' medical-assistant-app
```

#### Container Not Starting
If the container fails to start, check the logs for errors:
```bash
docker logs medical-assistant-app
```


## 📌 Option 2: Without Using Docker  <a name="manual-setup"></a>

### 1️⃣ Clone the Repository  
```bash  
git clone https://github.com/souvikmajumder26/Multi-Agent-Medical-Assistant.git  
cd Multi-Agent-Medical-Assistant  
```

### 2️⃣ Create & Activate Virtual Environment  
- If using conda:
```bash
conda create --name <environment-name> python=3.11
conda activate <environment-name>
```
- If using python venv:
```bash
python -m venv <environment-name>
source <environment-name>/bin/activate  # For Mac/Linux
<environment-name>\Scripts\activate     # For Windows  
```

### 3️⃣ Install Dependencies  

> [!IMPORTANT]  
> ffmpeg is required for speech service to work.

- If using conda:
```bash
conda install -c conda-forge ffmpeg
```
```bash
pip install -r requirements.txt  
```
- If using python venv:
```bash
winget install ffmpeg
```
```bash
pip install -r requirements.txt  
```

### 4️⃣ Set Up API Keys  
- Create a `.env` file and add the required API keys as shown in `Option 1`.

### 5️⃣ Run the Application  
- Run the following command in the activate environment.

```bash
python app.py
```
The application will be available at: [http://localhost:8000](http://localhost:8000)

### 6️⃣ Ingest additional data into the Vector DB
Run any one of the following commands as required.
- To ingest one document at a time:
```bash
python ingest_rag_data.py --file ./data/raw/brain_tumors_ucni.pdf
```
- To ingest multiple documents from a directory:
```bash
python ingest_rag_data.py --dir ./data/raw
```

---

## 🧠 Usage  <a name="usage"></a>

> [!NOTE]
> 1. The first run can be jittery and may get errors - be patient and check the console for ongoing downloads and installations.
> 2. On the first run, many models will be downloaded - yolo for tesseract ocr, computer vision agent models, cross-encoder reranker model, etc.
> 3. Once they are completed, retry. Everything should work seamlessly since all of it is thoroughly tested.

- Upload medical images for **AI-based diagnosis**. Task specific Computer Vision model powered agents - upload images from 'sample_images' folder to try out.
- Ask medical queries to leverage **retrieval-augmented generation (RAG)** if information in memory or **web-search** to retrieve latest information.  
- Use **voice-based** interaction (speech-to-text and text-to-speech).  
- Review AI-generated insights with **human-in-the-loop verification**.  
