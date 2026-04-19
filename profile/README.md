# .github
O1 Summit Hackathon Submission by Team 35 - Arun, Erina, Dipan


Complete App Preview : https://project-sirius.lovable.app/

# Sirius — Private, On-Device Health AI for iPhone & Mac

> Built in under 24 hours at the O1 Summit Hackathon.  
> Your health AI that never phones home.

---

## What is Sirius?

Sirius is a fully private AI health assistant for iPhone and Mac. It understands your lab reports, listens to your spoken questions, reads from Apple Health, and builds a persistent memory of your medical history — all without sending a single byte to any cloud server.

Every layer of the stack — speech recognition, document OCR, LLM inference, health memory, and device sync — runs locally on hardware you already own. No account required. No internet in the critical path. No external API ever sees your health data.

---

## The Problem

Every "private" health app you trust today runs on the cloud. And every cloud-based health AI has a leak, a partner, or a buyer waiting.

| Company | Incident | Scale |
|---|---|---|
| **Cerebral** | Shared mental health data with Google, Meta & TikTok for ad targeting | 3.1M+ patients |
| **Flo Health** | Shared menstrual cycle data with Facebook, Google & ad networks | 100M+ users |
| **GoodRx** | FTC fine for sharing prescription data with third-party advertisers | $1.5M settlement |
| **BetterHelp** | Shared mental health data with Facebook & Snapchat | $7.8M FTC settlement |

The pattern is the model. Cloud-first AI means your health data is the product. Sirius is built on the opposite assumption: **the safest place for your health data is your own pocket.**

---

## Core Features

### Voice & Text Chat
Talk to Sirius like a friend. Ask about medications, lab results, symptoms, drug interactions, or upcoming appointments. It replies in natural voice or streaming text with full markdown support. Everything is transcribed and processed entirely on-device using Apple's SpeechRecognizer framework and AVSpeechSynthesizer.

### Document Scanning & OCR
Point your camera at a lab report, prescription, or discharge note. VisionKit extracts every value, Gemma reads it, and the result is saved to your health memory. Supports camera capture and PDF/document uploads.

### Apple Health Integration
Sirius reads your vitals, body metrics, heart rate trends, biological sex, age, and menstrual cycle data directly from Apple Health — with your explicit permission. Nothing is written back or shared. The data stays in a read-only in-memory cache, refreshed once per launch, and fed directly into Gemma's context window to make every answer more personal.

### Health Memory
Sirius maintains a persistent, encrypted, on-device health memory powered by **HealthMemoryModule** — a custom SQLite store using GRDB.swift and AES-256-GCM field-level encryption. Every conversation turn is stored and semantically indexed using Apple's NLEmbedding framework (512-dimensional vectors). Medications, conditions, and allergies are extracted automatically from conversations using pattern recognition and stored as structured records.

Each Gemma prompt is prefixed with a dynamic context block assembled from your most relevant health memory entries — so answers grow more personal the more you use Sirius.

### Proactive Calendar Awareness
Sirius reads your calendar on-device using EventKit, automatically detects doctor and hospital visits, and surfaces a personalised preparation brief — grounded in your actual health memory. It tells you what to bring, what to ask, and what to flag, the day before your appointment.

### iPhone ↔ Mac Peer Sync
Pair your iPhone and Mac once with a 6-digit code. From that point on, your entire health memory syncs over Bluetooth or local Wi-Fi using Apple's MultipeerConnectivity framework — no internet, no server, no account. Sync is end-to-end encrypted using HKDF-SHA256 key derivation.

### Trusted Medical Web Search
When a question requires current clinical information, Sirius sanitises and de-identifies the query before routing it through TinyFish to retrieve public information from a curated whitelist of verified medical sources only:

FDA · NIH / NLM · MedlinePlus · CDC · WHO · Drugs.com · Mayo Clinic · ClinicalTrials.gov · PubMed · DailyMed

No personal identifiers ever leave your device.

---

### Key Technical Decisions

- **LLM**: Gemma 4 E2B (base) or E4B (Pro), running via LiteRTLM-Swift with on-device inference. First-token latency under 200ms.
- **Memory store**: SQLite via GRDB.swift with AES-256-GCM field-level encryption and HKDF-SHA256 key derivation. Hybrid retrieval using structured SQL queries + 512-dim NLEmbedding semantic search.
- **Sync transport**: MultipeerConnectivity with `MCPeerSyncTransport` implementing the `SyncTransport` protocol from HealthMemoryModule. HKDF-SHA256 derived keys from the 6-digit pairing code.
- **Privacy architecture**: Zero cloud dependencies in the health data path. The only outbound request is a sanitised, de-identified medical query via **TinyFish** for web-grounded answers.
- **HIPAA**: All data encrypted at rest (AES-256-GCM), no cloud storage, no PHI transmitted over the internet, device-level access control.

---

## Why It's Different

| Feature | ChatGPT Health | Perplexity Health | Apple Health + Siri | **Sirius** |
|---|---|---|---|---|
| AI processing location | Cloud (OpenAI) | Cloud (Perplexity) | Cloud (Apple) | **On-device (Gemma 4)** |
| Health data sent to server | Yes | Yes | Yes | **Never** |
| Persistent health memory | No | No | No | **Yes** |
| Cross-device sync | Via cloud | Via cloud | Via iCloud | **Peer-to-peer (no cloud)** |
| Works fully offline | No | No | Limited | **Yes** |
| Proactive calendar prep | No | No | Basic | **Yes** |
| Document OCR | No | No | No | **Yes** |
| "We won't sell your data" | Promise | Promise | Promise | **Architecturally impossible** |

The last row is the one that matters. Sirius can't sell what it never receives.

---

## Pricing

### Sirius — $4.99 / month
- Gemma 4 E2B, fully on-device
- AI health chat — text & voice
- Health memory — up to 500 entries
- Document scanning + OCR
- Verified medical web search (FDA, NIH, Mayo Clinic…)
- Proactive calendar appointment prep
- Single device (iPhone or Mac)

### Sirius Pro — $9.99 / month
Everything in Sirius, plus:
- Gemma 4 E4B — stronger reasoning and longer answers
- Apple Health integration (vitals, heart rate, sleep, cycle)
- iPhone ↔ Mac peer sync over Bluetooth
- Extended health memory — up to 10,000 entries
- Priority context window for richer, more personal conversations
- Premium TTS voices and enhanced voice pipeline
- Early access to new model releases

---

## Our Mission

To give every person the ability to understand their own health — without trading their privacy for the privilege.

The most personal data a human generates — their body, their conditions, their fears about tomorrow's appointment — should never have to live on someone else's server. Sirius is proof that intelligent, deeply personalised health AI can exist entirely on the device in your pocket.

No compromises. No exceptions.

---

## Requirements

- iPhone running iOS 17 or later
- macOS 14 or later (Mac companion app)

---
