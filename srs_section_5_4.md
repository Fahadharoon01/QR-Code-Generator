# 5.4 System Implementation

## 5.4.1 Development Languages and Technologies

The CareerKonnekt platform is built as a distributed multi-service architecture. Each service is implemented using the most appropriate language and framework for its specific responsibilities.

---

### 5.4.1.1 Frontend — JavaScript / Next.js 14

**Language:** JavaScript (ES2022+)

The frontend is developed entirely in JavaScript using **Next.js 14**, a React-based full-stack web framework by Vercel. Next.js was chosen for its App Router architecture, server-side rendering (SSR), and file-based routing system which significantly reduces boilerplate code.

| Technology | Version | Purpose |
|---|---|---|
| Next.js | 14.x | React framework, SSR, routing, API routes |
| React | 18.x | UI component library (bundled with Next.js) |
| Tailwind CSS | 3.x | Utility-first CSS framework for responsive UI |
| PostCSS | 8.x | CSS transformation pipeline used by Tailwind |
| ESLint | 9.x | Static code analysis and code quality enforcement |

**Key Frontend Libraries:**

| Library | Purpose |
|---|---|
| `livekit-client` | WebRTC client for joining real-time video interview rooms |
| `@livekit/components-react` | Pre-built React components for LiveKit video UI |
| `@livekit/components-styles` | Default styles for LiveKit React components |
| `axios` | HTTP client for API calls to Backend Server 1 and Server 2 |

The frontend communicates exclusively with Backend Server 1 (REST API) and Backend Server 2 (AI microservice) via HTTP/HTTPS requests using JWT bearer token authentication. The LiveKit Video Room component connects directly to LiveKit Cloud via WebRTC for peer-to-peer video during interviews.

---

### 5.4.1.2 Backend Server 1 — JavaScript / Node.js with Express

**Language:** JavaScript (ES Modules, Node.js 20+)

Backend Server 1 serves as the **primary REST API server** responsible for authentication, job management, application handling, interview scheduling, payments, and real-time notifications. It is implemented using **Express.js v5** running on **Node.js**.

| Technology | Version | Purpose |
|---|---|---|
| Node.js | 20.x | JavaScript runtime environment |
| Express.js | 5.x | Minimal web framework for REST API routing |
| Mongoose | 9.x | MongoDB Object Document Mapper (ODM) |
| MongoDB Atlas | Cloud | Primary NoSQL database for all application data |

**Authentication & Security:**

| Library | Purpose |
|---|---|
| `jsonwebtoken` | JWT generation and verification for stateless authentication |
| `bcryptjs` | Password hashing with salt rounds |
| `google-auth-library` | Verifying Google OAuth 2.0 ID tokens |
| `cookie-parser` | Parsing HTTP cookies |
| `cors` | Cross-Origin Resource Sharing middleware |

**External Service Integrations:**

| Library | Purpose |
|---|---|
| `stripe` | Payment processing, subscription management, and webhook handling |
| `cloudinary` | Cloud image storage, CDN delivery, and image transformations |
| `sharp` | Server-side image processing and resizing before upload |
| `nodemailer` | SMTP email sending (OTP, password reset, no-show alerts) |
| `resend` | Transactional email delivery API (alternative/fallback to nodemailer) |
| `livekit-server-sdk` | Server-side LiveKit room creation and access token issuance |
| `openai` | OpenAI API integration for AI text enhancement in profile services |
| `@google/generative-ai` | Google Gemini API for additional AI generation capabilities |
| `@vapi-ai/server-sdk` | Vapi AI voice agent server SDK |

**API Documentation & Utilities:**

| Library | Purpose |
|---|---|
| `swagger-jsdoc` | Auto-generates OpenAPI 3.0 specification from JSDoc comments |
| `swagger-ui-express` | Serves interactive Swagger UI at `/docs` endpoint |
| `pdfkit` | Programmatic PDF generation for reports and documents |
| `axios` | HTTP client for inter-service communication |
| `dotenv` | Environment variable management |
| `nodemon` | Automatic server restart during development |

**Scheduled Tasks (Cron Jobs):**
Backend Server 1 implements interval-based cron jobs using Node.js `setInterval` for:
- Expiring overdue AI interviews (every 1 hour)
- Detecting HR/applicant no-shows 15 minutes after scheduled time (every 5 minutes)
- Closing expired LiveKit video rooms (every 5 minutes)
- Enforcing the 2-strike no-show rule and auto-withdrawing applications

---

### 5.4.1.3 Backend Server 2 — Python / FastAPI (AI Microservice)

**Language:** Python 3.11+

Backend Server 2 is a dedicated **AI microservice** responsible for all AI-powered features including job recommendations, applicant matching, interview question generation, interview scoring, career guidance, and voice verification proxying. It is built using **FastAPI**, a modern asynchronous Python web framework.

| Technology | Version | Purpose |
|---|---|---|
| Python | 3.11+ | Primary language for AI/ML workloads |
| FastAPI | 0.115+ | Async REST API framework with automatic OpenAPI docs |
| Uvicorn | 0.32+ | ASGI server for running FastAPI in production |
| Pydantic | 2.10+ | Data validation and settings management |
| pydantic-settings | 2.6+ | Environment-based configuration management |

**AI / LLM Integration:**

| Library | Version | Purpose |
|---|---|---|
| `openai` | 1.57+ | OpenAI GPT-4o API for agent reasoning and text generation |
| `openai-agents` | 0.14+ | OpenAI Agents SDK for multi-agent orchestration |
| `pinecone` | 5.0+ | Pinecone vector database client for semantic search |
| `numpy` | 2.1+ | Numerical computations and embedding vector operations |

**AI Agents Implemented:**

| Agent | Responsibility |
|---|---|
| Job Recommendation Agent | Embeds applicant profile and performs vector similarity search in Pinecone to surface relevant job listings |
| Applicant Recommendation Agent | Embeds job requirements and searches Pinecone for matching candidate profiles |
| Interview Agent | Generates contextual interview questions tailored to the job description and candidate profile |
| Interview Scoring Agent | Evaluates candidate answers against expected criteria and produces scores with detailed feedback |
| Career Guidance Agent | Provides personalised career advice through a conversational OpenAI-powered chat agent |
| Voice Verification Proxy | Acts as an HTTP proxy forwarding voice enroll/verify requests to the HuggingFace Voice Service |

**Document Processing:**

| Library | Purpose |
|---|---|
| `pypdf` | PDF resume parsing and text extraction |
| `python-docx` | Word document (.docx) resume parsing |
| `Pillow` | Image processing utilities |

**HTTP & Networking:**

| Library | Purpose |
|---|---|
| `httpx` | Async HTTP client for inter-service communication |
| `python-multipart` | Multipart form data parsing for file uploads |

---

### 5.4.1.4 HuggingFace Voice Service — Python / FastAPI

**Language:** Python 3.11+
**Deployment:** HuggingFace Spaces (Docker SDK, Port 7860)

The Voice Service is a standalone **speaker verification microservice** deployed on HuggingFace Spaces. It is never called directly from the browser — Backend Server 2 proxies all requests to it as an internal service.

| Technology | Version | Purpose |
|---|---|---|
| FastAPI | Latest | REST API endpoints for enroll, verify, clear |
| Uvicorn | Latest | ASGI server |
| Docker | Latest | Containerisation for HuggingFace Spaces deployment |

**Voice / Audio Processing:**

| Library | Version | Purpose |
|---|---|---|
| `resemblyzer` | 0.1.1+ | Generalised End-to-End (GE2E) speaker encoder neural network |
| `numpy` | 2.1+ | Audio sample processing and cosine similarity computation |
| `librosa` | 0.10+ | Advanced audio loading, resampling, and feature extraction |
| `soundfile` | 0.12+ | Reading and writing audio files |
| `scipy` | 1.10+ | Signal processing utilities |
| `torch` | 2.0+ | PyTorch deep learning runtime required by Resemblyzer |
| `audioread` | 3.0+ | Multi-backend audio decoding |
| `soxr` | 0.3+ | High-quality audio resampling |
| `numba` | 0.56+ | JIT compilation for performance-critical audio operations |
| `scikit-learn` | 1.1+ | Supporting ML utilities |

**Voice Verification Logic:**
- Speaker embeddings are 256-dimensional vectors produced by the Resemblyzer GE2E model
- Identity verification uses cosine similarity with a threshold of **0.70**
- Multiple-speaker detection uses cross-frame pairwise similarity with a threshold of **0.52**
- Audio is processed in 2-second frames filtered by an RMS energy threshold of **0.002**
- Embeddings are stored in-memory per `interview_id` with client-side caching for cold-start recovery

---

## 5.4.2 Tools Used in System

### 5.4.2.1 Development Tools

| Tool | Category | Purpose |
|---|---|---|
| **Visual Studio Code** | IDE | Primary code editor used across all services with extensions for JavaScript, Python, ESLint, and Prettier |
| **Git** | Version Control | Distributed version control system for tracking all source code changes |
| **GitHub** | Repository Hosting | Remote code repository hosting, branch management, and collaboration |
| **npm** | Package Manager | Node.js package manager for frontend and Backend Server 1 dependencies |
| **uv** | Package Manager | Fast Python package manager used for Backend Server 2 dependency management (replaces pip) |
| **nodemon** | Dev Utility | Automatically restarts Node.js server on file changes during development |
| **Docker** | Containerisation | Containerises Backend Server 2 and HuggingFace Voice Service for consistent deployments |
| **Nixpacks** | Build System | Automatically detects and builds Backend Server 2 for Railway deployment |

---

### 5.4.2.2 API Development & Testing Tools

| Tool | Purpose |
|---|---|
| **Swagger UI** | Auto-generated interactive API documentation served at `/docs` on Backend Server 1. Developers can explore and test all REST endpoints directly in the browser |
| **swagger-jsdoc** | Parses JSDoc comments in route files to automatically generate the OpenAPI 3.0 specification |
| **FastAPI Swagger** | Backend Server 2 FastAPI automatically generates its own interactive API docs at `/docs` and ReDoc at `/redoc` |
| **Postman** | Manual API testing, collection management, and environment variable configuration for all three backend services |

---

### 5.4.2.3 Database & Storage Tools

| Tool | Category | Purpose |
|---|---|---|
| **MongoDB Atlas** | Cloud Database | Primary NoSQL document database. Stores users, applicants, companies, jobs, applications, interviews, notifications, and payment records |
| **Mongoose** | ODM | Object Document Mapper providing schema validation, middleware hooks, and query building for MongoDB |
| **Pinecone** | Vector Database | Cloud-hosted vector database storing 1536-dimensional OpenAI embeddings for job listings and applicant profiles. Enables semantic similarity search for AI recommendations |
| **Cloudinary** | Media Storage | Cloud-based image storage and CDN. Handles profile photo uploads, transformations, and fast delivery |

---

### 5.4.2.4 External Service & Integration Tools

| Tool / Service | Purpose |
|---|---|
| **Stripe** | Payment gateway for token plan purchases. Handles card processing, subscription management, and webhook event delivery for payment confirmation |
| **LiveKit Cloud** | Real-time video infrastructure providing WebRTC-based video interview rooms. Backend Server 1 creates rooms and issues access tokens; the browser connects directly via WebRTC |
| **OpenAI API** | Powers all AI agents in Backend Server 2 — job recommendations, applicant matching, interview question generation, interview scoring, and career guidance chat using GPT-4o and text-embedding-3-small |
| **Clipdrop API** | AI-powered background removal for profile photos. Called by Backend Server 1 profile services when an applicant uploads their photo |
| **Google OAuth 2.0** | Social sign-in provider. Frontend initiates OAuth flow; Backend Server 1 verifies the Google ID token using `google-auth-library` |
| **Nodemailer / Resend** | Transactional email delivery for OTP verification, password reset, interview reminders, and no-show alert emails |
| **HuggingFace Spaces** | Serverless container hosting platform for the Voice Service. Provides free GPU/CPU hosting for the Resemblyzer GE2E model with Docker SDK runtime |

---

### 5.4.2.5 Deployment & Infrastructure Tools

| Tool | Purpose |
|---|---|
| **Vercel** | Recommended deployment platform for the Next.js 14 frontend. Provides edge CDN, automatic HTTPS, and preview deployments per branch |
| **Railway** | Cloud deployment platform for Backend Server 1 (Node.js) and Backend Server 2 (FastAPI). Configuration managed via `railway.toml` |
| **Docker** | Container runtime for both Backend Server 2 and HuggingFace Voice Service, ensuring reproducible builds across environments |
| **Nixpacks** | Zero-configuration build system that auto-detects Python/Node.js projects and creates optimised Docker images for Railway |
| **HuggingFace Spaces** | Managed container platform for the Voice Service microservice. Handles scaling, networking, and model serving automatically |

---

### 5.4.2.6 Real-Time Communication Tools

| Tool | Purpose |
|---|---|
| **LiveKit Server SDK** | Server-side SDK used by Backend Server 1 to programmatically create interview rooms, issue participant tokens with permissions, and manage room lifecycle |
| **livekit-client** | Browser-side WebRTC SDK used by the Next.js frontend to join video rooms, publish audio/video tracks, and subscribe to remote participants |
| **Server-Sent Events (SSE)** | Lightweight unidirectional push technology implemented at `/api/sse` in Backend Server 1 for real-time notification delivery to connected browser clients without WebSocket overhead |
