---
title: "Generative AI Tennis Coach"
hideMeta: true
draft: false
hiddenInList: true
---

The **Generative AI Tennis Coach** is an intelligent sports-analysis system that unifies **machine learning**, **multimodal understanding**, and **retrieval-augmented generation (RAG)** to deliver **personalized, evidence-grounded coaching feedback** derived directly from match footage.

At its core, the platform implements a **Generative AI orchestration layer** built on **Google Vertex AI** and a **PostgreSQL + pgvector** knowledge base.
Every processed rally, pose sequence, and shot-classification embedding becomes part of a **temporal semantic graph**, enabling the model to retrieve contextually relevant insights when users chat with the AI coach.
Video analysis tasks run as **serverless Celery workers on GCP Cloud Run Jobs**, allowing elastic scaling and full isolation between inference and user data.

Built on a **FastAPI + Next.js** stack and containerized via **Docker Compose**, the system coordinates asynchronous job execution through **RabbitMQ + Celery**.
All video assets are persisted in **Cloudflare R2**, while structured metadata and vector embeddings reside in **PostgreSQL/pgvector**, supporting efficient semantic search and contextual reasoning across modalities.

---

## System Overview

### 1. Video Upload & Task Orchestration

Players upload videos through the web interface.
The backend queues Celery tasks that are executed as **Cloud Run Jobs**, each operating in an isolated container with authenticated APIs—enabling **horizontal scaling**, **fault isolation**, and **cost-efficient parallelism**.

### 2. AI-Driven Video Analysis Pipeline

* **Rally Detection:** Audio-frequency analysis of ball-impact sounds groups frames into rallies.
* **Shot Classification:** Pose-estimation and neural models categorize strokes (*forehand*, *backhand*, *serve*).
* **Cross-Validation:** Fuses audio and pose cues to reduce false positives.
* **Highlight Compilation:** Produces timestamped clips and categorized highlight reels.

### 3. Generative AI Coaching Engine

Processed clips and analytics are sent to **Gemini 2.5 via Vertex AI**, which produces **structured, timestamped JSON feedback** on tactics, positioning, and biomechanics.
These outputs are embedded into the **pgvector knowledge base**, forming a **long-term coaching memory** aligned with each player’s gameplay history.

### 4. Retrieval-Augmented Conversation

When users interact with the in-app AI chat, their queries are **semantically matched** against stored embeddings.
Relevant feedback snippets—with timestamps and tactical context—are **injected into Gemini’s prompt**, enabling **grounded, explainable responses** tied to the player’s own footage.

### 5. YouTube Recommendation Layer

The AI agent reformulates insights into contextual search queries (e.g., *“improve open-stance forehand”*), retrieves **short-form drills** and **long-form tutorials**, validates and caches results, and merges them with chat responses—creating a **multi-modal learning experience** that blends **analysis, explanation, and guided improvement**.


## Architecture Diagrams

### Main Node
<img 
  src="https://r2.eshafeeqe.space/works/es/diagrams/main.png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

### Worker Node

<img 
  src="https://r2.eshafeeqe.space/works/es/diagrams/worker.png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

### AI Chat FLow
<img 
  src="https://r2.eshafeeqe.space/works/es/diagrams/chat%20flow.png" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

Notes:
- Main node = single host running both Next.js (frontend) and FastAPI (backend).
- Workers are security-isolated; they never touch the DB directly and report only via authenticated FastAPI APIs.
- Direct-to-R2 upload minimizes main node bandwidth; workers pull from R2 for processing.
- RAG happens at chat time on the main node using pgvector; Gemini is used both post-processing (to create feedback)
  and at chat-time (to generate grounded replies).


## Demo videos and images


<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/es/Analysis%20Result.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


<video width="100%" style="max-height: 400px; object-fit: contain;" controls>
  <source src="https://r2.eshafeeqe.space/works/es/Recording%202025-02-27%20184305.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

<img 
  src="https://r2.eshafeeqe.space/works/es/1752571233888.jpeg" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

<img 
  src="https://r2.eshafeeqe.space/works/es/image.webp" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

<img 
  src="https://r2.eshafeeqe.space/works/es/5f4dc3a1-3983-41a3-8d8d-e376396f9882.webp" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

<img 
  src="https://r2.eshafeeqe.space/works/es/homepage.webp" 
  alt="Example image" 
  width="100%" 
  style="max-height: 400px; object-fit: contain;" 
/>

