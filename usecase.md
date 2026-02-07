# 📊 CareerKonnekt - Complete System Use Case Analysis

## 🔍 System Analysis Summary

I've thoroughly analyzed your complete codebase (Server-1, Server-2, Frontend) and mapped all features against your Functional Requirements. Here's the comprehensive use case breakdown:

---

## 👤 APPLICANT / USER USE CASES

### **UC1: User Registration & Authentication**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System

**Preconditions:** None

**Main Flow:**
1. User navigates to signup page
2. User selects "Applicant" role
3. User enters: email, password, name, phone, location, current role, skills, experience
4. System validates data and creates user account
5. System sends OTP to email for verification
6. User enters OTP to verify account
7. System activates account

**Alternative Flows:**
- 3a. Invalid email format → System shows error
- 5a. Email delivery fails → User can resend OTP
- 6a. Invalid/expired OTP → User must request new OTP

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/applicantController.js` (registerApplicant)
  - `models/auth/userAuthModel.js` (User schema)
  - `models/auth/applicantAuthModel.js` (Applicant schema)
  - `services/nodeMailer.js` (Email verification)
  - `routes/auth/applicantRoutes.js`

---

### **UC2: User Login**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System

**Preconditions:** User must be registered and email verified

**Main Flow:**
1. User enters email and password
2. System validates credentials
3. System checks email verification status
4. System generates JWT token
5. System returns token and user data
6. User redirected to dashboard

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/authController.js` (loginUser)
  - `middlewares/userAuth.js` (Token verification)

---

### **UC3: Password Reset**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System

**Main Flow:**
1. User clicks "Forgot Password"
2. User enters registered email
3. System sends OTP to email
4. User enters OTP and new password
5. System validates OTP and updates password

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/authController.js` (sendResetOTP, resetPassword)

---

### **UC4: Manage Profile (Create/Update)**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System, AI Service (Server-2)

**Preconditions:** User must be logged in

**Main Flow:**
1. User navigates to profile section
2. User fills/updates profile information:
   - Personal Info (name, email, phone, location, LinkedIn, GitHub, portfolio)
   - Professional Summary
   - Education (degree, institution, field, dates, description)
   - Experience (job title, company, dates, description)
   - Projects (name, description, technologies, link)
   - Skills (technical & soft skills)
3. System validates all fields
4. System saves profile to MongoDB
5. System checks if profile is complete (has education/experience + skills)
6. **If profile complete:** System sends profile data to Server-2 (AI Service)
7. Server-2 converts profile to vector embeddings
8. Server-2 stores in Pinecone for job matching
9. System returns success

**Alternative Flows:**
- 3a. Validation fails → Show specific error messages
- 6a. AI service unavailable → Profile still saved, AI sync attempted later

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/profileController.js` (createOrUpdateProfile)
  - `models/applicant/profileModel.js` (Profile schema)
  - `services/aiRecommendationService.js` (ingestProfileToAI)
  - `routes/applicant/profileRoutes.js`
- **Server-2 Files:**
  - `routes/profiles/profile_routes.py` (POST /api/profiles/ingest)
  - `services/profile_service.py` (ingest_profile)
  - `utils/embeddings.py` (prepare_profile_text_for_embedding)

---

### **UC5: Upload Profile Picture**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System, Cloudinary

**Main Flow:**
1. User selects profile picture
2. System validates image format (jpg, png, jpeg)
3. System uploads to Cloudinary
4. System saves image URL to profile
5. Returns updated profile

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/profileController.js` (uploadProfilePicture)
  - `config/cloudinary.js`

---

### **UC6: AI-Enhanced Profile Content**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System, Gemini AI

**Preconditions:** User has profile with content to enhance

**Main Flow:**
1. User clicks "AI Enhance" on any section:
   - Professional Summary
   - Education Description
   - Experience Description
   - Project Description
2. System sends content to Gemini AI
3. Gemini AI enhances content (professional language, better structure)
4. System returns enhanced text
5. User can accept or reject enhancement

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/profileController.js`:
    - aiEnhanceSummary
    - aiEnhanceEducation
    - aiEnhanceExperience
    - aiEnhanceProject
  - `services/aiEnhancementService.js` (Gemini AI integration)

---

### **UC7: Check Profile Completion Status**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System

**Main Flow:**
1. System checks profile for:
   - Personal information complete
   - At least one education OR experience entry
   - At least one skill
2. Returns completion percentage and missing sections

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/profileController.js` (checkProfileCompletion)
  - `middlewares/profileCompletion.js`

---

### **UC8: Design & Download CV/Resume**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System

**Preconditions:** Profile must be completed

**Main Flow:**
1. User selects CV template:
   - Minimal
   - Modern
   - Classic
   - Professional
   - Creative
   - Minimal with Image
2. User selects accent color
3. User clicks "Download Resume"
4. System generates PDF using selected template
5. System returns PDF for download

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/profileController.js` (downloadResume)
  - `utils/pdfGenerator.js`
  - `utils/templates/` (6 different templates)

---

### **UC9: Get AI-Powered Job Recommendations**
**Status:** ✅ **Fully Implemented**

**Actors:** Applicant, System, AI Service (Server-2)

**Preconditions:** User must have completed profile synced to AI service

**Main Flow:**
1. User navigates to job recommendations section
2. User optionally sets filters (location, job type, etc.)
3. Frontend calls Server-1 recommendations API
4. Server-1 forwards request to Server-2
5. Server-2 retrieves applicant's profile vector from Pinecone
6. Server-2 searches for similar job vectors using cosine similarity
7. Server-2 performs skill matching:
   - Compares applicant skills with job required skills
   - Calculates matched skills count
8. Server-2 ranks jobs by:
   - Similarity score (AI-based semantic matching)
   - Skill match count
9. Server-2 returns top N recommendations (default 10)
10. Server-1 forwards to Frontend
11. User sees personalized job list with:
    - Job title, company, location
    - Similarity score
    - Matched skills highlighted
    - Job type, experience level

**Alternative Flows:**
- 5a. Profile not found in AI system → Sync profile first
- 6a. No active jobs available → Return empty list with message

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/applicant/recommendationController.js` (getRecommendations)
  - `routes/applicant/recommendationRoutes.js`
  - `services/aiRecommendationService.js` (getJobRecommendations)
- **Server-2 Files:**
  - `routes/recommendations/recommendation_routes.py`
  - `services/profile_service.py` (get_job_recommendations)
  - `services/job_service.py` (search_similar_jobs)
  - `config/pinecone_config.py` (Vector database)

---

### **UC10: View Job Details**
**Status:** 🟡 **Partially Implemented** (Backend ready, Frontend integration needed)

**Actors:** Applicant, System

**Main Flow:**
1. User clicks on a job from recommendations
2. System fetches complete job details
3. System displays:
   - Job description
   - Required skills
   - Responsibilities
   - Qualifications
   - Salary range
   - Deadline
4. User can apply for job

---

### **UC11: Apply for Job**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Applicant, System

**Preconditions:** Profile must be completed

**Main Flow:**
1. User views job details
2. User clicks "Apply Now"
3. System attaches user's profile/resume
4. User optionally adds cover letter
5. System submits application
6. System notifies company
7. User receives application confirmation

**Database Schema Required:** Job Applications model

---

### **UC12: Track Application Status**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Applicant, System

**Main Flow:**
1. User navigates to "My Applications"
2. System displays all applications with status:
   - Pending
   - Shortlisted
   - Interview Scheduled
   - Rejected
   - Hired
3. User can view application details

---

### **UC13: AI-Based Interview Preparation**
**Status:** 🔵 **Planned / Not Implemented** (FR4)

**Actors:** Applicant, System, AI Service (Server-2)

**Preconditions:** User has applied for a job

**Main Flow:**
1. User selects job/role for interview prep
2. System analyzes job requirements and user profile
3. AI generates relevant interview questions:
   - Technical questions based on required skills
   - Behavioral questions
   - Role-specific scenarios
4. User practices answering questions
5. AI evaluates responses:
   - Content quality
   - Completeness
   - Confidence indicators
6. System provides feedback and improvement suggestions
7. User can retry questions

**Implementation Required:**
- **Server-2:** 
  - Interview question generation service
  - Response evaluation using NLP
  - Feedback generation
- **Database:** Interview practice sessions

---

### **UC14: Career Guidance & Skill Gap Analysis**
**Status:** 🔵 **Planned / Not Implemented** (FR5)

**Actors:** Applicant, System, AI Service (Server-2)

**Main Flow:**
1. User requests career guidance
2. System analyzes:
   - Current skills
   - Experience level
   - Industry trends
   - Target job requirements
3. AI identifies skill gaps
4. System recommends:
   - Skills to learn
   - Courses/certifications
   - Career paths
   - Target roles
5. User can save career roadmap
6. System tracks progress over time

**Implementation Required:**
- **Server-2:**
  - Skill gap analysis engine
  - Career path recommendation system
  - Industry trends database/API
  - Learning resource integration

---

### **UC15: Receive Notifications**
**Status:** 🔵 **Planned / Not Implemented** (FR7)

**Actors:** Applicant, System

**Main Flow:**
1. System monitors events:
   - New job matches available
   - Application status updates
   - Interview invitations
   - Profile completion reminders
2. System sends notifications via:
   - Email
   - In-app notifications
3. User views and manages notifications

---

### **UC16: Participate in Automated AI Interview**
**Status:** 🔵 **Planned / Not Implemented** (FR11 - Applicant side)

**Actors:** Applicant, System, AI Service (Server-2)

**Preconditions:** User has been shortlisted for a job

**Main Flow:**
1. User receives interview invitation
2. User joins interview at scheduled time
3. System presents AI-generated questions one by one
4. User records video/audio responses OR types answers
5. AI analyzes responses in real-time:
   - Content accuracy
   - Communication clarity
   - Technical correctness
   - Behavioral indicators
6. System submits responses to company
7. User receives interview completion confirmation

**Implementation Required:**
- **Server-2:**
  - Interview orchestration service
  - Multi-modal response analysis (text, audio, video)
  - Real-time evaluation engine
- **Frontend:**
  - Video/audio recording interface
  - Real-time question display

---

## 🏢 COMPANY USE CASES

### **UC17: Company Registration**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Representative, System

**Main Flow:**
1. Company rep navigates to signup
2. Selects "Company" role
3. Enters company details:
   - Company name
   - Email
   - Password
   - Phone number
   - Website
   - Location
   - Company size
   - Industry
4. System validates data
5. System creates company account
6. System sends verification email with OTP
7. Company rep verifies email
8. Account activated

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/companyController.js` (registerCompany)
  - `models/auth/companyAuthModel.js`
  - `routes/auth/companyRoutes.js`

---

### **UC18: Company Login**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System

**Implementation Details:**
- Same as UC2 (uses unified login endpoint)
- `controllers/auth/authController.js` (loginUser)

---

### **UC19: Get Company Profile**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System

**Main Flow:**
1. Company rep logs in
2. System retrieves company profile
3. Displays: company name, email, location, size, industry, etc.

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/companyController.js` (getCompanyProfile)

---

### **UC20: Update Company Profile**
**Status:** 🟡 **Partially Implemented** (Route exists, full update logic needed)

**Actors:** Company Rep, System

**Main Flow:**
1. Company rep edits profile information
2. System validates changes
3. System updates company record
4. Returns updated profile

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/auth/companyController.js` (updateCompanyProfile)

---

### **UC21: Create Job Post**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System, AI Service (Server-2)

**Preconditions:** 
- Company must be logged in
- Email must be verified

**Main Flow:**
1. Company rep clicks "Post New Job"
2. Fills job details:
   - Title
   - Department
   - Location
   - Job Type (Full-time, Part-time, Contract, Freelance, Internship)
   - Work Mode (Remote, On-site, Hybrid)
   - Experience Required
   - Salary Range (min, max)
   - Number of Positions
   - Application Deadline
   - Job Description
   - Responsibilities
   - Requirements
   - Benefits
   - Required Skills (comma-separated)
3. Optionally saves as draft
4. System validates all fields:
   - Required fields present
   - Salary max >= salary min
   - Deadline is future date
   - Job type is valid
   - Description length >= 50 chars
5. System saves job to MongoDB with status:
   - "draft" if saved as draft
   - "active" if published
6. **If published (not draft):**
   - System sends job to Server-2 (AI Service)
   - Server-2 converts job to vector embedding
   - Server-2 stores in Pinecone for matching
7. Job is now live and searchable

**Alternative Flows:**
- 3a. Save as draft → Job not sent to AI service
- 4a. Validation fails → Show specific errors
- 6a. AI service unavailable → Job still posted, AI sync attempted later

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/company/jobPostController.js` (createJobPost)
  - `models/company/jobPostModel.js`
  - `services/aiRecommendationService.js` (ingestJobToAI)
  - `routes/company/jobRoutes.js`
- **Server-2 Files:**
  - `routes/jobs/job_routes.py` (POST /api/jobs/ingest)
  - `services/job_service.py` (ingest_job)
  - `utils/embeddings.py` (prepare_job_text_for_embedding)

---

### **UC22: View All Company Jobs**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System

**Main Flow:**
1. Company rep navigates to "My Jobs"
2. System fetches all jobs for this company
3. Displays jobs grouped by status:
   - Active
   - Draft
   - Closed
   - Deleted (soft-deleted, not shown to applicants)

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/company/jobPostController.js` (getCompanyJobs)

---

### **UC23: View Job Details**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System

**Main Flow:**
1. Company rep clicks on a job
2. System retrieves complete job details
3. Displays all job information

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/company/jobPostController.js` (getJobById)

---

### **UC24: Update Job Post**
**Status:** ✅ **Fully Implemented**

**Actors:** Company Rep, System, AI Service (Server-2)

**Main Flow:**
1. Company rep edits job information
2. System validates changes
3. System updates job in MongoDB
4. **If job status is "active":**
   - System sends updated job to Server-2
   - Server-2 regenerates embeddings
   - Server-2 updates vector in Pinecone
5. Returns updated job

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/company/jobPostController.js` (updateJobPost)
  - `services/aiRecommendationService.js` (updateJobInAI)
- **Server-2 Files:**
  - `routes/jobs/job_routes.py` (PUT /api/jobs/update)
  - `services/job_service.py` (update_job)

---

### **UC25: Delete Job Post**
**Status:** ✅ **Fully Implemented** (Soft Delete)

**Actors:** Company Rep, System, AI Service (Server-2)

**Main Flow:**
1. Company rep clicks "Delete Job"
2. System confirms action
3. System performs soft delete (sets is_deleted=true)
4. System sends delete request to Server-2
5. Server-2 removes job vector from Pinecone
6. Job no longer appears in recommendations

**Alternative Flows:**
- Job remains in company's view (marked as deleted)
- Can potentially be restored

**Implementation Details:**
- **Server-1 Files:**
  - `controllers/company/jobPostController.js` (deleteJobPost)
  - `services/aiRecommendationService.js` (deleteJobFromAI)
- **Server-2 Files:**
  - `routes/jobs/job_routes.py` (DELETE /api/jobs/delete)
  - `services/job_service.py` (delete_job)

---

### **UC26: View Job Applications**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Company Rep, System

**Preconditions:** Job must be posted and have applications

**Main Flow:**
1. Company rep opens a job post
2. System displays all applications for this job
3. Shows applicant summary:
   - Name
   - Skills
   - Experience
   - Match score (from AI)
4. Company can filter/sort applications

---

### **UC27: AI-Assisted Applicant Shortlisting**
**Status:** 🔵 **Planned / Not Implemented** (FR10)

**Actors:** Company Rep, System, AI Service (Server-2)

**Main Flow:**
1. Company rep views applications for a job
2. System displays AI-generated match scores
3. AI automatically identifies top candidates based on:
   - Skill match percentage
   - Experience relevance
   - Education alignment
   - Career trajectory
4. System suggests shortlist
5. Company can:
   - Accept AI suggestions
   - Manually adjust shortlist
   - Add/remove candidates
6. System marks selected candidates as "Shortlisted"
7. System notifies shortlisted candidates

**Implementation Required:**
- **Server-2:**
  - Enhanced ranking algorithm
  - Shortlisting recommendation service
  - Candidate scoring system
- **Database:**
  - Application status tracking
  - Shortlist records

---

### **UC28: Schedule AI Interviews**
**Status:** 🔵 **Planned / Not Implemented** (FR11)

**Actors:** Company Rep, System, AI Service (Server-2)

**Preconditions:** Candidates must be shortlisted

**Main Flow:**
1. Company rep selects shortlisted candidates
2. Company configures interview:
   - Interview type (Technical, HR, Both)
   - Duration
   - Question difficulty
   - Custom questions (optional)
   - Deadline for completion
3. System schedules automated AI interviews
4. AI generates interview questions based on:
   - Job requirements
   - Candidate profile
   - Company preferences
5. System sends interview invitations to candidates
6. Candidates complete interviews
7. AI evaluates responses
8. System generates interview reports

**Implementation Required:**
- **Server-2:**
  - Interview scheduling service
  - Question bank / generation system
  - Interview evaluation engine
  - Report generation
- **Database:**
  - Interview sessions
  - Interview responses
  - Evaluation scores

---

### **UC29: View Interview Results & Evaluations**
**Status:** 🔵 **Planned / Not Implemented** (FR11)

**Actors:** Company Rep, System

**Main Flow:**
1. Company rep navigates to interview results
2. System displays for each candidate:
   - Overall score
   - Technical competency score
   - Communication score
   - Behavioral assessment
   - Question-wise breakdown
   - Video/audio recordings (if enabled)
   - AI-generated insights
3. Company can:
   - Compare candidates
   - Export reports
   - Make hiring decisions

---

### **UC30: Select & Hire Candidates**
**Status:** 🔵 **Planned / Not Implemented** (FR12)

**Actors:** Company Rep, System

**Main Flow:**
1. Company reviews all candidate data:
   - Profile
   - Application
   - AI match score
   - Interview evaluation
2. Company selects candidate(s) for hiring
3. System updates candidate status to "Selected"
4. System sends offer notification to candidate
5. System updates other candidates:
   - Rejected candidates notified
   - Job closed if all positions filled
6. System archives hiring data for future reference

---

### **UC31: Company Verification**
**Status:** 🔵 **Planned / Not Implemented** (FR8)

**Actors:** Admin, System

**Main Flow:**
1. Admin reviews company registration
2. Admin verifies:
   - Company legitimacy
   - Business documents
   - Contact information
3. Admin approves/rejects company
4. System updates company verification status
5. Only verified companies can post jobs

---

## 👨‍💼 ADMIN USE CASES

### **UC32: Admin Login**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin enters credentials
2. System validates admin role
3. Grants access to admin dashboard

**Implementation Required:**
- Admin user creation in database
- Admin role check middleware
- Admin dashboard routes

---

### **UC33: Verify User Accounts**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin views pending user verifications
2. Admin reviews user information
3. Admin approves/rejects accounts
4. System notifies users

---

### **UC34: Verify Company Accounts**
**Status:** 🔵 **Planned / Not Implemented** (FR8)

**Actors:** Admin, System

**Main Flow:**
1. Admin views pending company registrations
2. Admin checks:
   - Business registration documents
   - Company website
   - Contact legitimacy
3. Admin verifies or rejects
4. System updates company status

---

### **UC35: Monitor Platform Activity**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin views dashboard with metrics:
   - Total users (applicants + companies)
   - Active jobs
   - Applications submitted
   - Interview completion rate
   - System performance
2. Admin generates reports
3. Admin identifies issues/trends

---

### **UC36: Manage Jobs (Remove Inappropriate)**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin reviews reported/flagged jobs
2. Admin verifies content
3. Admin can:
   - Remove job
   - Warn company
   - Ban company (serious violations)
4. System notifies company

---

### **UC37: Oversee AI System Performance**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin monitors AI services:
   - Pinecone vector database status
   - OpenAI API usage
   - Recommendation accuracy
   - Interview evaluation quality
2. Admin views AI metrics:
   - Match success rate
   - Interview completion rate
   - User satisfaction scores
3. Admin can adjust AI parameters

**Implementation Required:**
- Monitoring dashboard
- AI metrics collection
- Performance analytics

---

### **UC38: Manage System Configuration**
**Status:** 🔵 **Planned / Not Implemented**

**Actors:** Admin, System

**Main Flow:**
1. Admin accesses configuration panel
2. Admin can modify:
   - AI service settings
   - Email templates
   - Platform policies
   - Feature flags
3. System validates changes
4. System applies configuration

---

## 📊 SUMMARY TABLE

| Use Case ID | Use Case Name | Actor | Status |
|-------------|---------------|-------|---------|
| **APPLICANT** |
| UC1 | User Registration & Authentication | Applicant | ✅ Implemented |
| UC2 | User Login | Applicant | ✅ Implemented |
| UC3 | Password Reset | Applicant | ✅ Implemented |
| UC4 | Manage Profile (Create/Update) | Applicant | ✅ Implemented |
| UC5 | Upload Profile Picture | Applicant | ✅ Implemented |
| UC6 | AI-Enhanced Profile Content | Applicant | ✅ Implemented |
| UC7 | Check Profile Completion Status | Applicant | ✅ Implemented |
| UC8 | Design & Download CV/Resume | Applicant | ✅ Implemented |
| UC9 | Get AI-Powered Job Recommendations | Applicant | ✅ Implemented |
| UC10 | View Job Details | Applicant | 🟡 Partially Implemented |
| UC11 | Apply for Job | Applicant | 🔵 Planned |
| UC12 | Track Application Status | Applicant | 🔵 Planned |
| UC13 | AI-Based Interview Preparation | Applicant | 🔵 Planned |
| UC14 | Career Guidance & Skill Gap Analysis | Applicant | 🔵 Planned |
| UC15 | Receive Notifications | Applicant | 🔵 Planned |
| UC16 | Participate in Automated AI Interview | Applicant | 🔵 Planned |
| **COMPANY** |
| UC17 | Company Registration | Company | ✅ Implemented |
| UC18 | Company Login | Company | ✅ Implemented |
| UC19 | Get Company Profile | Company | ✅ Implemented |
| UC20 | Update Company Profile | Company | 🟡 Partially Implemented |
| UC21 | Create Job Post | Company | ✅ Implemented |
| UC22 | View All Company Jobs | Company | ✅ Implemented |
| UC23 | View Job Details | Company | ✅ Implemented |
| UC24 | Update Job Post | Company | ✅ Implemented |
| UC25 | Delete Job Post | Company | ✅ Implemented |
| UC26 | View Job Applications | Company | 🔵 Planned |
| UC27 | AI-Assisted Applicant Shortlisting | Company | 🔵 Planned |
| UC28 | Schedule AI Interviews | Company | 🔵 Planned |
| UC29 | View Interview Results & Evaluations | Company | 🔵 Planned |
| UC30 | Select & Hire Candidates | Company | 🔵 Planned |
| UC31 | Company Verification | Company | 🔵 Planned |
| **ADMIN** |
| UC32 | Admin Login | Admin | 🔵 Planned |
| UC33 | Verify User Accounts | Admin | 🔵 Planned |
| UC34 | Verify Company Accounts | Admin | 🔵 Planned |
| UC35 | Monitor Platform Activity | Admin | 🔵 Planned |
| UC36 | Manage Jobs | Admin | 🔵 Planned |
| UC37 | Oversee AI System Performance | Admin | 🔵 Planned |
| UC38 | Manage System Configuration | Admin | 🔵 Planned |

---

## 🎯 IMPLEMENTATION STATUS BREAKDOWN

### ✅ Fully Implemented (16 Use Cases)
Authentication, Profile Management, Job Posting, AI Job Recommendations, CV Design

### 🟡 Partially Implemented (2 Use Cases)
Job viewing (frontend needed), Company profile updates

### 🔵 Planned / Not Implemented (20 Use Cases)
Job applications, AI interviews, Interview preparation, Career guidance, Admin features

---

## 📋 UML-READY USE CASE STRUCTURE

For creating **Use Case Diagrams**, actors are:
1. **Applicant/User**
2. **Company Representative**
3. **Admin**
4. **AI Service (Server-2)** - Secondary actor
5. **External Services** (OpenAI, Pinecone, Cloudinary) - Secondary actors

**Include Relationships:**
- UC4 includes UC5 (profile update can include picture upload)
- UC21 includes calling AI service for job ingestion

**Extend Relationships:**
- UC6 extends UC4 (AI enhancement is optional during profile creation)
- UC13 extends UC16 (interview prep extends actual interview)

---

## 💡 FOR FUTURE DIAGRAM REQUESTS

Complete understanding of:

✅ **Implemented Features:**
- Authentication flow (signup, login, OTP verification)
- Profile management with AI enhancement
- Job posting with AI vectorization
- Job recommendation using Pinecone + OpenAI
- CV generation with multiple templates
- Microservices communication (Server-1 ↔ Server-2)

✅ **Planned Features:**
- Job application workflow
- AI interview system
- Career guidance
- Admin panel
- Notifications

✅ **Technical Architecture:**
- Server-1: Node.js/Express with MongoDB
- Server-2: FastAPI/Python with Pinecone & OpenAI
- Vector embeddings for semantic job matching
- Microservices REST API communication

---

## 📐 Ready for Diagram Generation

This use case documentation is prepared for creating:
1. **Use Case Diagrams** - Visual representation of all actors and use cases
2. **Sequence Diagrams** - Detailed interaction flows (e.g., job recommendation, AI interview)
3. **Activity Diagrams** - Complex workflow visualizations
4. **Class Diagrams** - Data model relationships

All diagrams will be based on this comprehensive system analysis, combining both implemented and planned features for a complete FYP presentation.
