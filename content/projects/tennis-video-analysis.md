---
title: "Tennis Video Analysis"
hideMeta: true
draft: false
hiddenInList: true
---

The platform combines machine learning and audio processing to analyze tennis gameplay videos. It is built on a FastAPI backend with a Next.js frontend and uses PostgreSQL for persistent storage. Distributed task processing is handled through RabbitMQ and Celery. The entire system runs in Docker Compose, with multiple containerized services and Cloudflare R2 for video storage. Users upload videos through the web interface, and processing occurs asynchronously across worker nodes that extract rallies, classify shots, generate highlights, and produce AI-based coaching feedback.

The system uses a security-isolated architecture where the video processing worker operates in a separate container without direct database access. Communication between the worker and the backend occurs through authenticated HTTP API endpoints using internal API keys. This design supports deploying workers on independent infrastructure while maintaining security and scalability. RabbitMQ serves as the message broker, decoupling video uploads from processing tasks and enabling asynchronous execution. When a user uploads a video, the frontend enqueues a task in Celery, which is picked up by worker nodes that process the video and report progress through dedicated API endpoints.


The processing pipeline is organized into sequential stages. Rally detection applies audio frequency analysis to identify ball impact sounds and groups them into rally segments based on timing. Shot classification uses pose estimation to extract player movement data from video frames, which are then analyzed by a neural network to classify each shot. The system cross-validates vision-based detections with audio-based hits to reduce false positives. Rally generation applies a quality filter, including only segments with multiple validated shots, before compiling the final rally videos. The highlight generator produces several clip types, such as forehand and backhand collections, longest rallies by hit count, and fastest rallies by hit density, each limited to a short duration for review.

The platform integrates Google Gemini 2.5 to provide contextual tennis coaching. During processing, analyzed videos are uploaded to Gemini’s Files API. The system uses two structured prompts: one for strategic analysis, covering rally structure, positioning, and movement efficiency, and another for technical analysis, focusing on shot mechanics such as grip, stance, and footwork. Gemini returns structured JSON feedback with timestamped observations, which is stored in the database. When users interact with the chat service, this feedback is included in the system prompt, allowing Gemini to provide specific advice referencing moments from the user’s own footage.

A YouTube recommendation component enhances the coaching feedback by linking technique issues to tutorial videos. The system converts identified technical problems into search queries, such as mapping grip-related issues to “tennis grip fix.” It applies a dual recommendation strategy: short “Quick Fix” videos for immediate improvement and longer “Deep Dive” tutorials for more detailed learning. Recommendations are preloaded and cached to optimize API usage. Each video suggestion is validated for duration and deduplicated across multiple searches to ensure relevance and variety. These recommendations are integrated into the AI chat responses, with cached results serving as a fallback if real-time queries fail.

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

