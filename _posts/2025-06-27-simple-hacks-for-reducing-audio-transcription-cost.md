---
layout: post
title: >
    Simple hacks for reducing audio transcription cost.
tags: [ffmpeg, gen-ai]
description: >
    Learning from processing real estate call center data
---

I recently concluded a subcontracted project for one of the top real estate builders in the Middle East. 

With the advent of viral real estate listings on social media comes a barrage of call center inquiries about the projects and their details. The UAE, in particular, receives calls from across the globe.

The client was already using an AI SaaS product that would take call recordings and extract:

- Entities:
  - Name
  - Nationality
  - Property, builder, or area of interest
- Budget
- Purchase timeline

The system also tagged phone numbers and other metadata, automatically raising CRM tickets. These tickets were routed to agents over WhatsApp within seconds, along with a dossier containing extracted details, a transcript, and a playable audio file link. This enabled agents to exercise judgment and prioritize callbacks effectively.

The pricing with the SaaS provider was a fixed cost of 7 AED per call. My task was to replicate this functionality at a lower cost, while ensuring the client’s in-house tech team could take over and fully own the resulting IP.

## Audio Processing

Using `ffmpeg`, we processed each audio file to:

- Standardize the format to FLAC  
- Reduce the bitrate to 128 kbps  
- Speed up the playback to 1.25x–1.5x  

Running `ffprobe` on the optimized files showed that we reduced audio file sizes by approximately 60% on average. This resulted in downstream savings in both storage and processing costs.

## Audio-to-Text Conversion

After experimenting with optimized versions of OpenAI Whisper and various Hugging Face models, I ultimately chose the reliable Google Speech-to-Text API. One key reason was the need to handle multi-lingual conversations — many East Asian callers switched between their native language and English, e.g. Hindi/Urdu -> English.

Google's API allowed us to specify a primary language along with up to four alternative languages.

Another useful trick was hosting audio files in Google Cloud Storage. This appeared to speed up transcription time — my hunch is that Google avoids copying files already hosted within their cloud infrastructure (as opposed to fetching from S3 or a local server).

I handed over a Dockerized version of the pipeline, micro-batching audio files in 3-minute windows, and ran a worker pool of 20 concurrent workers. `ffmpeg` was the most CPU-intensive part of the process. The client now runs the entire pipeline within Kubernetes, where the number of messages in Redis streams dynamically determines the scaling of the worker pool.

Their cost is now equal to the reserved instance + google transcription cost + Gemini 2.5 Pro. When spread across the no of calls they are now saving approx. 92% in cost in June alone.

Tech stack:
- Redis Streams for Pub/Sub
- Python
  - DSPy for interaction with LLMs
- OpenRouter
- ffmpeg
- Google
  - Cloud Storage
  - Gemini 2.5 Pro
  - Speech-to-text
---