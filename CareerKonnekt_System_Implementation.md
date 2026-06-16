# CareerKonnekt - System Implementation

---

## Table of Contents
1. [6.1 System Implementation](#61-system-implementation)
2. [6.2 Development Languages and Technologies](#62-development-languages-and-technologies)
   - [6.2.1 Next.js](#621-nextjs)
   - [6.2.2 Node.js](#622-nodejs)
   - [6.2.3 Express.js](#623-expressjs)
   - [6.2.4 FastAPI & Python AI Agents](#624-fastapi--python-ai-agents)
   - [6.2.5 MongoDB](#625-mongodb)
   - [6.2.6 Tailwind CSS](#626-tailwind-css)
   - [6.2.7 JWT (JSON Web Tokens)](#627-jwt-json-web-tokens)
3. [6.3 Tools](#63-tools)
4. [6.4 Collaboration/Sequence Diagrams](#64-collaboration-sequence-diagrams)
5. [6.5 Conclusion](#65-conclusion)

---

## 6.1 System Implementation
CareerKonnekt is implemented as a **microservices-based web application** with two backend services and a single-page frontend. The system is designed following modern software architecture patterns and best practices.

The implementation follows a **3-tier architecture**:
1. **Presentation Tier**: Next.js frontend
2. **Application Tiers**:
   - Server 1 (Node.js/Express) - Core application logic
   - Server 2 (FastAPI/Python) - AI & recommendation engine
3. **Data Tiers**:
   - MongoDB - Primary database for Server 1
   - PostgreSQL - Database for Server 2
   - Pinecone - Vector database for embeddings

### System Implementation Flow
The system implements end-to-end features:

1. **User Registration & Authentication** → JWT-based authentication
2. **Profile Management** → Applicant/Company profile creation with AI profile background enhancement
3. **Resume Upload & Parsing** → OpenAI-powered parsing of PDF/DOCX resumes
4. **Resume Templates & AI Enhancement** → 7+ professional templates with AI-powered content suggestions
5. **Job Posting & Management** → Company can create, edit, and delete job posts which are ingested into Pinecone vector DB
6. **Job Search & Recommendations** → AI-powered semantic search + personalized recommendations
7. **Application Process** → One-click job applications with status tracking
8. **Interview Scheduling** → AI and live interview scheduling
9. **AI Interviews** → Voice/video interviews with facial and tone analysis using ElevenLabs/VAPI + face-api.js
10. **Live Interviews** → Real-time video interviews using LiveKit
11. **Career Guidance** → OpenAI-powered chat agent for personalized career advice
12. **Notifications** → Real-time updates on applications, interviews, and status
13. **Payments & Subscriptions** → Premium features via payment integration

---

## 6.2 Development Languages and Technologies

### 6.2.1 Next.js
**Role**: Frontend framework for building the user interface

**Version**: 16.1.6
**Key Features Implemented**:
- **App Router**: File-based routing for clean URL structure
- **Server & Client Components**: Hybrid rendering approach
- **Image Optimization**: Next.js Image component for efficient image delivery
- **Static & Dynamic Rendering**: Optimized page loading
- **API Routes**: Proxy calls to backend services
- **State Management**: Context API for application state (authentication, user data, etc.)

**Frontend Dependencies**:
```json
{
  "react": "19.2.3",
  "react-dom": "19.2.3",
  "next": "16.1.6",
  "framer-motion": "^12.30.0",
  "lucide-react": "^0.563.0",
  "livekit-client": "^2.19.2",
  "@livekit/components-react": "^2.9.21",
  "face-api.js": "^0.22.2",
  "react-toastify": "^11.0.5",
  "sweetalert2": "^11.26.18",
  "jspdf": "^4.1.0",
  "axios": "^1.13.4"
}
```

**Key Pages & Components**:
- Authentication pages (`signin`, `signup/user`, `signup/company`, `forgot-password`, `reset-password`, `verify-email`)
- Applicant dashboard, profile, job search
- Company dashboard, job posting, applicant management
- Interview room components (both AI and live)
- Resume builder with templates

---

### 6.2.2 Node.js
**Role**: Runtime environment for Server 1 (core backend)

**Version**: Node.js 18+
**Key Responsibilities**:
1. Core application logic and business rules
2. Authentication and authorization
3. User and company profile management
4. Job postings and application management
5. Interview scheduling
6. Notification system
7. Payment and subscription handling

**Key Server 1 Dependencies**:
- `express` - Web framework
- `mongoose` - MongoDB ODM
- `jsonwebtoken` - JWT authentication
- `bcrypt` - Password hashing
- `nodemailer` - Email services
- `cloudinary` - Media storage
- `multer` - File uploads
- `cors` - CORS policy handling
- `dotenv` - Environment variables

---

### 6.2.3 Express.js
**Role**: Web application framework for Server 1

**Express.js Architecture in CareerKonnekt**:
Server 1 follows an MVC (Model-View-Controller) pattern:
- **Models**: Mongoose schemas for MongoDB collections
- **Controllers**: Request handlers with business logic
- **Routes**: API endpoint definitions
- **Services**: Reusable business logic modules
- **Middlewares**: Authentication, error handling, etc.
- **Config**: Database, Cloudinary, email configuration

**Server 1 Project Structure**:
```
CareerKonnekt-Backend-Server-1/
├── config/
│   ├── cloudinary.js
│   ├── mongodb.js
│   └── nodeMailer.js
├── controllers/
│   ├── applicant/
│   ├── auth/
│   ├── company/
│   ├── interview/
│   ├── notification/
│   ├── payment/
│   └── public/
├── middlewares/
│   ├── profileCompletion.js
│   └── userAuth.js
├── models/
│   ├── applicant/
│   ├── auth/
│   ├── company/
│   ├── interview/
│   └── notification/
├── routes/
│   ├── applicant/
│   ├── auth/
│   ├── company/
│   ├── interview/
│   ├── notification/
│   ├── payment/
│   └── public/
├── services/
│   ├── aiEnhancementService.js
│   ├── aiRecommendationService.js
│   ├── emailNotificationService.js
│   ├── profileImageBackgroundService.js
│   └── tokenService.js
├── utils/
│   ├── templates/
│   ├── cronJobs.js
│   └── pdfGenerator.js
├── server.js
├── package.json
└── README.md
```

---

### 6.2.4 FastAPI & Python AI Agents
**Role**: AI microservice (Server 2) for recommendation, parsing, and AI agents

**Tech Stack for Server 2**:
```
- Python 3.14+
- FastAPI + Uvicorn
- OpenAI SDK
- Pinecone
- pypdf + python-docx
- Pydantic v2 + pydantic-settings
```

#### Key Server 2 Features:
1. **OpenAI Integration (text-embedding-3-small, GPT models):
   - Embedding generation for jobs and profiles
   - Resume parsing from PDF/DOCX
   - Career guidance chat
   - Interview agent
   - Interview scoring agent

2. **Pinecone Vector Search**:
   - Stores embeddings for jobs and profiles in separate namespaces
   - Semantic similarity search for job and applicant recommendations
   - Hybrid scoring of similarity + skill matching

3. **ElevenLabs & VAPI Integration**:
   - Voice synthesis for AI interviews
   - Voice agent interactions
   - Natural conversation flow

**Server 2 Project Structure**:
```
CareerKonnekt-Backend-Server-2/
├── src/
│   └── my_app/
│       ├── agents/
│       │   └── openai/
│       │       ├── applicant_recommendation_agent.py
│       │       ├── career_guidance_agent.py
│       │       ├── document_parsing_agent.py
│       │       ├── interview_agent.py
│       │       ├── interview_scoring_agent.py
│       │       └── job_recommendation_agent.py
│       ├── config/
│       │   ├── pinecone_config.py
│       │   └── settings.py
│       ├── database/
│       │   └── postgresql/
│       ├── models/
│       ├── routes/
│       │   ├── agents/
│       │   ├── jobs/
│       │   ├── profiles/
│       │   └── recommendations/
│       ├── schemas/
│       │   ├── career_guidance_schemas.py
│       │   ├── interview_schemas.py
│       │   ├── job_schemas.py
│       │   ├── profile_schemas.py
│       │   └── resume_schemas.py
│       ├── services/
│       │   ├── job_parser_service.py
│       │   ├── job_service.py
│       │   ├── profile_service.py
│       │   └── resume_parser_service.py
│       ├── utils/
│       │   └── embeddings.py
│       └── main.py
├── startup.py
├── pyproject.toml
└── README.md
```

#### OpenAI API Key Usage:
The OpenAI API key is used across multiple AI agent endpoints:
```python
OPENAI_API_KEY=sk-...
OPENAI_CHAT_MODEL=gpt-4o-mini
OPEN_EMBEDDING_MODEL=text-embedding-3-small
```
Agents that use OpenAI:
- `document_parsing_agent` - Extract structured data from resumes
- `career_guidance_agent` - Personalized career advice
- `interview_agent` - AI interview questions and interaction
- `interview_scoring_agent` - Score and evaluate interviews
- `job_recommendation_agent` - Job recommendations
- `applicant_recommendation_agent` - Candidate recommendations for jobs

---

### 6.2.5 MongoDB
**Role**: Primary NoSQL database for Server 1

**Key Collections & Schemas**:
1. **ApplicantAuth**:
   ```javascript
   {
     fullName: String,
     email: String (unique),
     password: String (bcrypt hashed),
     phone: String,
     location: String,
     profilePicture: String,
     isVerified: Boolean,
     isProfileCompleted: Boolean,
     createdAt: Date,
     updatedAt: Date
   }
   ```

2. **CompanyAuth**:
   ```javascript
   {
     companyName: String,
     email: String (unique),
     password: String (bcrypt hashed),
     companySize: String,
     industry: String,
     website: String,
     location: String,
     logo: String,
     isVerified: Boolean,
     createdAt: Date,
     updatedAt: Date
   }
   ```

3. **ApplicantProfile**:
   ```javascript
   {
     applicantId: ObjectId (ref: ApplicantAuth),
     resume: String,
     skills: [String],
     experience: [{
       title: String,
       company: String,
       startDate: Date,
       endDate: Date,
       description: String
     }],
     education: [{...}],
     projects: [{...}],
     createdAt: Date,
     updatedAt: Date
   }
   ```

4. **JobPost**:
   ```javascript
   {
     title: String,
     companyId: ObjectId (ref: CompanyAuth),
     description: String,
     requirements: [String],
     responsibilities: [String],
     location: String,
     salary: { min: Number, max: Number },
     skills: [String],
     employmentType: String,
     experienceLevel: String,
     status: String (ACTIVE/CLOSED),
     applicantsCount: Number,
     createdAt: Date,
     updatedAt: Date
   }
   ```

5. **JobApplication**:
   ```javascript
   {
     jobId: ObjectId (ref: JobPost),
     applicantId: ObjectId (ref: ApplicantAuth),
     status: String (APPLIED/SHORTLISTED/REJECTED/INTERVIEW),
     appliedAt: Date,
     updatedAt: Date
   }
   ```

6. **Interview**:
   ```javascript
   {
     jobId: ObjectId (ref: JobPost),
     applicantId: ObjectId (ref: ApplicantAuth),
     companyId: ObjectId (ref: CompanyAuth),
     type: String (AI/LIVE),
     status: String (SCHEDULED/COMPLETED/CANCELLED),
     scheduledAt: Date,
     livekitRoomId: String,
     recordingUrl: String,
     score: Number,
     assessment: Object,
     createdAt: Date
   }
   ```

7. **Notification**:
   ```javascript
   {
     userId: ObjectId,
     userType: String,
     type: String,
     title: String,
     message: String,
     isRead: Boolean,
     createdAt: Date
   }
   ```

---

### 6.2.6 Tailwind CSS
**Role**: Utility-first CSS framework for styling the frontend

**Tailwind Version**: 4
**Key Features**:
- Responsive design with `sm`, `md`, `lg`, `xl`, `2xl` breakpoints
- Dark/light theme support
- Component composition with utility classes
- Custom color scheme with `#091e40` as primary brand color
- Animation utilities combined with Framer Motion
- Clean, maintainable styling without custom CSS files

---

### 6.2.7 JWT (JSON Web Tokens)
**Role**: Authentication and session management

**JWT Implementation**:
1. **Access Token**:
   - Short-lived token for API authorization
   - Expires after 15-30 minutes
   - Stored in memory or HTTP-only cookies
   
2. **Refresh Token**:
   - Long-lived token for getting new access tokens
   - Expires after 7-30 days
   - Stored in HTTP-only secure cookies

**JWT Flow**:
1. User logs in → Server generates access & refresh tokens
2. Access token sent in `Authorization: Bearer <token>` header with each API request
3. Middleware `userAuth.js` verifies JWT signature and decodes user information
4. If access token expires → Refresh token is used to get a new access token

---

## 6.3 Tools

| Category | Tools Used |
|----------|------------|
| **Code Editors & IDEs** | VS Code, PyCharm |
| **Version Control** | Git, GitHub |
| **Package Managers** | npm (Node.js), uv/pip (Python) |
| **API Testing** | Postman, Swagger UI (FastAPI) |
| **Database** | MongoDB Compass, pgAdmin |
| **Design & Prototyping** | Figma, Canva |
| **Cloud Services** | Vercel (Frontend), Railway, AWS (Backends), Cloudinary (Storage) |
| **Monitoring & Logging** | Winston, Pino, CloudWatch |
| **CI/CD** | GitHub Actions |
| **Diagramming** | PlantUML (activity, sequence, use case diagrams) |
| **AI/ML Services** | OpenAI Platform, Pinecone Console, ElevenLabs, Vapi |

---

## 6.4 Collaboration/Sequence Diagrams

CareerKonnekt includes detailed PlantUML diagrams for all major workflows:

### 6.4.1 User Authentication Sequence (SequenceDiagrams/01_User_Authentication.puml)
- User registration flow (email/password & Google)
- Login & JWT token issuance
- Email verification OTP flow
- Password reset flow

### 6.4.2 Applicant Profile Creation (SequenceDiagrams/02_Applicant_Profile_Creation.puml)
- Profile information input
- Resume upload and AI parsing
- AI profile picture background enhancement
- Profile completion check

### 6.4.3 Company Job Posting (SequenceDiagrams/03_Company_Job_Posting.puml)
- Job creation by company
- Job ingestion into Pinecone (embedding generation via Server 2)
- Job publishing

### 6.4.4 Job Application Process (SequenceDiagrams/04_Job_Application_Process.puml)
- Applicant job search & application
- Application status management
- Notification triggers

### 6.4.5 AI Interview Process (SequenceDiagrams/05_AI_Interview_Process.puml)
- Interview initialization
- OpenAI interview agent interaction
- ElevenLabs voice synthesis
- Facial expression analysis via face-api.js
- Interview scoring & assessment

### 6.4.6 LiveKit HR Interview (SequenceDiagrams/06_HR_Video_Interview_LiveKit.puml)
- Interview scheduling
- LiveKit room creation
- Real-time video call flow
- Recording & analysis

### 6.4.7 Job Recommendations (SequenceDiagrams/07_Job_Recommendations.puml)
- Profile embedding in Pinecone
- Semantic similarity search
- Skill matching & scoring
- Recommendation delivery

### 6.4.8 Applicant Recommendations (SequenceDiagrams/08_Applicant_Recommendations.puml)
- Job embedding
- Candidate search & ranking
- Shortlisting

### 6.4.9 Career Guidance Chat (SequenceDiagrams/09_Career_Guidance_Chat.puml)
- User query input
- OpenAI career guidance agent response
- Context management

### 6.4.10 Payment/Subscription (SequenceDiagrams/10_Payment_Subscription.puml)
- Subscription selection
- Payment flow
- Premium feature activation

### 6.4.11 Notification System (SequenceDiagrams/11_Notification_System.puml)
- Notification triggers
- Email & in-app notifications
- Status tracking

### 6.4.12 Resume Parsing (SequenceDiagrams/12_Resume_Parsing.puml)
- PDF/DOCX resume upload
- Text extraction
- OpenAI parsing to structured data
- Profile auto-fill

---

## 6.5 Conclusion
The CareerKonnekt system is a robust, scalable, and feature-rich platform implemented using cutting-edge technologies. Key strengths include:

1. **Microservices Architecture**: Separation of concerns with Server 1 and Server 2, allowing independent scaling and development.
2. **AI/ML Integration**: Comprehensive use of OpenAI, Pinecone, and facial analysis tools for intelligent features.
3. **Modern Stack**: Next.js, Node.js, FastAPI, and MongoDB provide a solid foundation for development and scalability.
4. **Security**: JWT, bcrypt, and industry-standard security practices.
5. **Professional Design**: Tailwind CSS + Framer Motion for a beautiful, responsive UI.
6. **End-to-End Features**: Complete recruitment lifecycle from job search to hiring.

This implementation enables CareerKonnekt to revolutionize how job seekers find opportunities and employers hire talent!
