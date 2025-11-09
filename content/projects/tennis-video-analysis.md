---
title: "Generative AI Tennis Coach"
hideMeta: true
draft: false
hiddenInList: true
---

The **Generative AI Tennis Coach** is an intelligent tennis-analysis platform that unifies **machine learning**, **audio-visual understanding**, and **retrieval-augmented generation (RAG)** to deliver **personalized coaching feedback** grounded in real match footage.

Built on a **FastAPI + Next.js** stack, the platform runs fully containerized via **Docker Compose**, coordinating asynchronous processing through **RabbitMQ + Celery** workers.  
Video assets are stored in **Cloudflare R2**, while structured metadata, feedback, and vector embeddings reside in **PostgreSQL with pgvector** for efficient retrieval and contextual reasoning.

---

## System Overview

### 1. Video Upload & Task Queue

Players upload tennis videos through the web interface.  
The backend queues processing tasks in **Celery**, which are executed by isolated worker containers.  
Each worker communicates securely via authenticated API endpoints—enabling **horizontal scaling** and **strict data separation** between inference and user data.

---

### 2. AI-Driven Video Analysis Pipeline

- **Rally Detection:** Audio-frequency analysis detects ball-impact sounds and groups them into rallies.  
- **Shot Classification:** Pose-estimation and neural-network models classify strokes (*forehand*, *backhand*, *serve*).  
- **Cross-Validation:** Combines audio and visual cues to reduce false positives.  
- **Highlight Compilation:** Generates categorized rally clips and short highlight reels.

---

### 3. Generative AI Coaching Engine

Processed clips are sent to **Google Gemini 2.5**, which produces **structured, timestamped JSON feedback** on tactics, positioning, and technique.  
Each observation is embedded into a **vector database** (pgvector), building a **temporal knowledge base** aligned with the analyzed footage.

---

### 4. Retrieval-Augmented Conversation

When users interact with the in-app AI chat, their queries are **semantically matched** against the stored embeddings.  
Relevant coaching insights—complete with timestamps and context—are injected into Gemini’s prompt, allowing the model to respond with **highly specific, grounded feedback** linked to moments from the user’s own footage.

---

### 5. YouTube Recommendation Layer

The AI agent transforms coaching insights into contextual search queries (e.g., *“improve open-stance forehand”*) and retrieves:

- **Quick Fix** videos — short, actionable clips for immediate improvement  
- **Deep Dive** tutorials — long-form breakdowns for technical learning  

Results are validated, cached, and merged with AI chat responses for a **multi-modal learning experience** that blends analysis, explanation, and guided improvement.

---

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

