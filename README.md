# SecOps-Assistant

This repository contains the implementation of an intelligent security operation assistant designed to aid security teams in handling incidents rapidly and accurately. The system utilizes a **Stateful Retrieval-Augmented Generation (RAG)** architecture, enabling the assistant to maintain continuous conversational context.

## Project Background

In cybersecurity operations, Standard Operating Procedure (SOP) documents are often dense and complex. During active incidents, manually searching through PDF documents for specific protocols is time-consuming and prone to human error. This project bridges that gap by transforming static SOP documents into an interactive system capable of providing instant diagnoses, mitigation steps, and precise Command Line Interface (CLI) instructions.

## Key Features

### 1. Stateful Memory Management
Unlike standard RAG systems that are stateless, this assistant integrates **Astra DB Chat Memory**. This allows the system to store and retrieve previous conversation history, enabling users to ask follow-up questions without needing to re-establish the context of the incident.

### 2. Guardrails and Anti-Hallucination
The system is configured with a strict similarity threshold for vector searches. If the requested information is not found within the SOP database, the AI automatically triggers a standard fallback response to escalate the issue to the L3 Security Team, preventing the generation of inaccurate or "hallucinated" technical advice.

### 3. Linux Forensics Optimization
The assistant is specifically optimized for Linux environments. It generates investigation commands based on the **Order of Volatility (OoV)** principle, covering memory analysis, running processes, active network connections, and filesystem artifacts.

### 4. Indicators of Compromise (IOC) Identification
The assistant identifies and provides mitigation guidance for specific Linux Indicators of Compromise, such as persistence mechanisms (cron jobs, systemd services) and suspicious network activity based on the uploaded reference documentation.

## Technical Stack

* **Orchestration Logic:** Langflow (Low-code AI orchestration).
* **Large Language Model:** Google Gemma 3 Series.
* **Vector Database:** DataStax Astra DB (Serverless Vector Search).
* **Persistent Memory:** Astra DB Chat Memory Integration.
* **Embedding Model:** Google Generative AI Embeddings.

## System Architecture

The system workflow is divided into two primary pipelines:

1.  **Ingestion Pipeline:** Security SOP documents in PDF format are partitioned into chunks, converted into vector embeddings, and stored permanently in Astra DB.
2.  **Inference Pipeline:** When a user submits a query, the system performs a vector search in Astra DB to find the most relevant context, retrieves chat history from the persistent memory, and merges them into a prompt template before processing by the Gemini model.

## Getting Started

### System Prerequisites
* Langflow installation via browser or docker.
* An active Astra DB account with an Application Token.
* Google AI API Key.

### Installation Steps
1.  **Clone this repository:**
    ```bash
    git clone [https://github.com/himendrafadh/SecOps-Assistant.git](https://github.com/himendrafadh/SecOps-Assistant.git)
    ```
2.  **Locate the flow files:**
    Navigate to the `/flows` directory within this repository.
3.  **Import to Langflow:**
    Import the provided JSON file into your Langflow interface.
4.  **Configuration:**
    Configure environment variables (API Keys and Tokens) on each relevant node (Astra DB and Google Generative AI nodes).
5.  **Data Ingestion:**
    Upload your SOP documentation into the **File** node, then initiate the data ingestion process by clicking the **Play** button on the Astra DB Ingestion node.

---

**Developed by Himendra Fadhil as a Capstone Project for the Hacktiv8 X IBM SkillsBuild Program.**
