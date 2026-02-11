# CareerKonnekt Frontend Documentation

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Technology Stack](#technology-stack)
- [Project Architecture](#project-architecture)
- [Directory Structure](#directory-structure)
- [Environment Configuration](#environment-configuration)
- [Routing & Navigation](#routing--navigation)
- [API Services](#api-services)
- [State Management](#state-management)
- [Authentication System](#authentication-system)
- [Core Components](#core-components)
- [Pages Overview](#pages-overview)
- [Utilities](#utilities)
- [Getting Started](#getting-started)
- [Development Guide](#development-guide)

---

## 🎯 Project Overview

**CareerKonnekt** is an AI-powered job recruitment platform built with Next.js 16 (App Router). The application serves two primary user types:
- **Applicants/Job Seekers**: Create profiles, browse jobs, get AI-powered recommendations, and apply to positions
- **Companies/Recruiters**: Post jobs, manage applications, view analytics, and leverage AI for job description generation

The frontend communicates with two backend services:
- **Server-1 (Node.js/Express)**: Main backend API (Port 4000) - handles authentication, user management, job postings
- **Server-2 (Python/FastAPI)**: AI/ML service (Port 8000/8001) - handles recommendations, profile analysis, AI enhancements

---

## 🛠 Technology Stack

### Core Framework
- **Next.js 16.1.6** (App Router) - React framework with server-side rendering
- **React 19.2.3** - UI library
- **React DOM 19.2.3** - React rendering

### Styling
- **Tailwind CSS 4** - Utility-first CSS framework
- **@tailwindcss/postcss** - PostCSS plugin for Tailwind
- **Custom CSS** - Global styles and animations

### Animation & UI
- **Framer Motion 12.30.0** - Advanced animations and gestures
- **Lenis 1.3.17** - Smooth scrolling library
- **Three.js 0.182.0** - 3D graphics (for interactive elements)
- **Maath 0.10.8** - Math utilities for 3D
- **Lucide React 0.563.0** - Icon library

### State & Data Management
- **React Context API** - Global state management
- **Axios 1.13.4** - HTTP client for API calls

### UI Feedback
- **React Toastify 11.0.5** - Toast notifications
- **SweetAlert2 11.26.18** - Beautiful alert modals

### Document Generation
- **html2canvas 1.4.1** - HTML to canvas conversion
- **jsPDF 4.1.0** - PDF generation (resume download)

### Development Tools
- **ESLint 9** - Code linting
- **PostCSS** - CSS processing
- **Babel React Compiler 1.0.0** - Optimizations

---

## 🏗 Project Architecture

### Architecture Pattern
The application follows a **modular component-based architecture** with clear separation of concerns:

```
Frontend (Next.js)
    ├── Presentation Layer (Components)
    ├── Business Logic (Context + Hooks)
    ├── Data Layer (API Services)
    └── Utilities (Helpers & Validators)
```

### Communication Flow
```
User Interface ⟷ React Components ⟷ Context (State) ⟷ API Services ⟷ Backend Servers
                                                            ├── Server-1 (Main API)
                                                            └── Server-2 (AI/ML)
```

---

## 📁 Directory Structure

```
Frontend/CareerKonnekt-Frontend/
│
├── .next/                          # Next.js build output (auto-generated)
├── node_modules/                   # Dependencies
├── public/                         # Static assets (images, fonts, etc.)
│
├── src/                           # Source code
│   ├── app/                       # Next.js App Router pages
│   │   ├── (admin)/               # Admin routes (empty - future implementation)
│   │   ├── (auth)/                # Authentication pages
│   │   ├── (landing)/             # Landing page
│   │   ├── applicant/             # Applicant dashboard & pages
│   │   ├── company/               # Company dashboard & pages
│   │   ├── cookies/               # Cookie policy page
│   │   ├── globals.css            # Global styles
│   │   ├── layout.js              # Root layout
│   │   ├── page.js                # Home page (redirects to landing)
│   │   └── providers.jsx          # Context providers wrapper
│   │
│   ├── assets/                    # Static assets (images, data)
│   │   ├── Bg_Images/             # Background images
│   │   ├── landingPage/           # Landing page images
│   │   ├── logo/                  # Logo files
│   │   └── staticData/            # Static data (FAQs, etc.)
│   │
│   ├── components/                # Reusable components
│   │   ├── common/                # Shared components
│   │   ├── landing/               # Landing page components
│   │   └── main-app/              # Main application components
│   │       ├── admin/             # Admin components
│   │       ├── applicant/         # Applicant-specific components
│   │       └── company/           # Company-specific components
│   │
│   ├── config/                    # Configuration files (empty)
│   │
│   ├── context/                   # React Context
│   │   └── AppContext.jsx         # Global application context
│   │
│   ├── services/                  # API services
│   │   └── api/                   # API modules
│   │       ├── auth_api/          # Authentication APIs
│   │       ├── company_api/       # Company APIs
│   │       ├── job_api/           # Job APIs
│   │       ├── profile_api/       # Profile APIs
│   │       └── recommendation_api/# AI recommendation APIs
│   │
│   └── utils/                     # Utility functions
│       ├── cookiesManager.js      # Cookie management
│       ├── tokenManager.js        # JWT token management
│       └── validation.js          # Form validation utilities
│
├── .env.local                     # Environment variables (not in git)
├── .gitignore                     # Git ignore rules
├── eslint.config.mjs              # ESLint configuration
├── jsconfig.json                  # JavaScript configuration
├── next.config.mjs                # Next.js configuration
├── package.json                   # Dependencies & scripts
├── package-lock.json              # Dependency lock file
├── postcss.config.mjs             # PostCSS configuration
└── README.md                      # Project README
```

---

## 🔐 Environment Configuration

### `.env.local`
```env
NEXT_PUBLIC_BACKEND_URL="http://localhost:4000"      # Main API server (Node.js)
NEXT_PUBLIC_API_URL="http://localhost:4000"          # Alias for backend URL
NEXT_PUBLIC_AI_BACKEND_URL="http://localhost:8001"   # AI/ML server (Python FastAPI)
```

**Purpose of Each Variable:**
- `NEXT_PUBLIC_BACKEND_URL`: Main backend API for auth, jobs, profiles
- `NEXT_PUBLIC_API_URL`: Alternative reference to main backend
- `NEXT_PUBLIC_AI_BACKEND_URL`: AI server for recommendations and enhancements

**Note**: The `NEXT_PUBLIC_` prefix makes these variables accessible in the browser.

---

## 🧭 Routing & Navigation

### Next.js App Router Structure

#### Route Groups (Parentheses)
Routes wrapped in `()` don't add a URL segment but allow logical grouping:

**1. `(admin)/`** - Future admin panel (empty)

**2. `(auth)/`** - Authentication pages
- `/signin` - User/Company login
- `/signup/user` - Applicant registration
- `/signup/company` - Company registration
- `/forgot-password` - Password reset request
- `/reset-password` - Password reset with token
- `/verify-email` - Email verification

**3. `(landing)/`** - Marketing pages
- `/home` - Landing page (marketing site)

#### Protected Routes

**Applicant Routes** (`/applicant/*`)
- `/applicant/dashboard` - Applicant dashboard with stats
- `/applicant/jobs` - Browse available jobs
- `/applicant/jobs/[jobId]` - Job details & application
- `/applicant/profile` - Create/edit resume profile

**Company Routes** (`/company/*`)
- `/company/dashboard` - Company dashboard with analytics
- `/company/jobs` - Manage posted jobs
- `/company/post-job` - Create new job posting
- `/company/applicants` - View applicants
- `/company/interviews` - Schedule interviews
- `/company/settings` - Company settings

#### Special Pages
- `/cookies` - Cookie policy page
- `/` (root) - Redirects to landing page

### Layout System

**Root Layout** (`src/app/layout.js`)
- Wraps entire application
- Sets up font (Poppins)
- Provides toast notifications
- Includes SmoothScroll wrapper
- Sets up Providers (Context)

**Applicant Layout** (`src/app/applicant/layout.jsx`)
- Wraps all `/applicant/*` routes
- Uses `<ApplicantLayout>` component
- Contains sidebar navigation
- Protected by authentication

**Company Layout** (`src/app/company/layout.jsx`)
- Wraps all `/company/*` routes
- Uses `<CompanyLayout>` component
- Contains sidebar navigation
- Protected by authentication

---

## 🌐 API Services

All API services are located in `src/services/api/`. Each service uses Axios with interceptors for authentication.

### 1. Authentication API (`auth_api.js`)

**Base URL**: `http://localhost:4000`

#### Endpoints:

##### `loginUser(credentials)`
```javascript
// POST /api/auth/login
// Request: { email, password }
// Response: { success, token, refreshToken, user, company }
```
**Purpose**: User/company authentication. Returns JWT token and user data.

##### `registerApplicant(userData)`
```javascript
// POST /api/auth/register-applicant
// Request: { name, email, phoneNo, location, currentRole, password, confirmPassword }
// Response: { success, token, user }
```
**Purpose**: Register new job seeker account.

##### `registerCompany(companyData)`
```javascript
// POST /api/auth/register-company
// Request: { companyName, companyEmail, phoneNo, website, location, companySize, industry, password, confirmPassword }
// Response: { success, token, company }
```
**Purpose**: Register new company/recruiter account.

##### `sendVerifyOTP(email)`
```javascript
// POST /api/auth/send-verify-otp
// Request: { email }
// Response: { success, message }
```
**Purpose**: Send email verification OTP.

##### `verifyEmail(email, otp)`
```javascript
// POST /api/auth/verify-email
// Request: { email, otp }
// Response: { success, message }
```
**Purpose**: Verify email with OTP code.

##### `sendResetOTP(email)`
```javascript
// POST /api/auth/send-reset-otp
// Request: { email }
// Response: { success, message }
```
**Purpose**: Send password reset OTP to email.

##### `verifyResetOTP(email, otp)`
```javascript
// POST /api/auth/verify-reset-otp
// Request: { email, otp }
// Response: { success, resetToken }
```
**Purpose**: Verify password reset OTP.

##### `resetPassword(resetToken, newPassword)`
```javascript
// POST /api/auth/reset-password
// Request: { resetToken, newPassword, confirmPassword }
// Response: { success, message }
```
**Purpose**: Reset password with verified token.

##### `logoutUser()`
```javascript
// POST /api/auth/logout
// Response: { success, message }
```
**Purpose**: Logout user and invalidate token.

##### `isAuthenticated()`
```javascript
// GET /api/auth/is-authenticated
// Response: { authenticated, user }
```
**Purpose**: Check if current session is valid.

---

### 2. Job API (`job_api.js`)

**Base URL**: `http://localhost:4000`

#### Company Job Management:

##### `createJobPost(jobData, isDraft)`
```javascript
// POST /api/company/jobs
// Request: { title, description, location, jobType, salary, requirements, responsibilities, skills, experienceLevel, isDraft }
// Response: { success, job }
```
**Purpose**: Create new job posting. Can save as draft.

##### `getCompanyJobs()`
```javascript
// GET /api/company/jobs
// Response: { success, jobs: [] }
```
**Purpose**: Get all jobs posted by logged-in company.

##### `getCompanyJobById(jobId)`
```javascript
// GET /api/company/jobs/:jobId
// Response: { success, job }
```
**Purpose**: Get single job details for editing.

##### `updateCompanyJob(jobId, jobData)`
```javascript
// PUT /api/company/jobs/:jobId
// Request: { ...updatedFields }
// Response: { success, job }
```
**Purpose**: Update existing job posting.

##### `deleteCompanyJob(jobId)`
```javascript
// DELETE /api/company/jobs/:jobId
// Response: { success, message }
```
**Purpose**: Delete job posting permanently.

#### Applicant Job Browsing:

##### `getAllJobs(filters)`
```javascript
// GET /api/jobs?search=&location=&jobType=&experienceLevel=
// Response: { success, jobs: [], total, page, pages }
```
**Purpose**: Browse all published jobs with optional filters.

##### `getJobById(jobId)`
```javascript
// GET /api/jobs/:jobId
// Response: { success, job }
```
**Purpose**: View single job details (public).

##### `applyToJob(jobId, applicationData)`
```javascript
// POST /api/jobs/:jobId/apply
// Request: { coverLetter, resumeUrl }
// Response: { success, application }
```
**Purpose**: Submit job application.

##### `getMyApplications()`
```javascript
// GET /api/applicant/applications
// Response: { success, applications: [] }
```
**Purpose**: Get all applications submitted by applicant.

##### `getApplicationStatus(applicationId)`
```javascript
// GET /api/applicant/applications/:applicationId
// Response: { success, application, status }
```
**Purpose**: Check status of specific application.

---

### 3. Profile API (`profile_api.js`)

**Base URL**: `http://localhost:4000`

#### Applicant Profile:

##### `getProfile()`
```javascript
// GET /api/applicant/profile
// Response: { success, profile }
```
**Purpose**: Get applicant's resume/profile data.

##### `createOrUpdateProfile(profileData)`
```javascript
// POST /api/applicant/profile
// Request: {
//   personalInfo: { fullName, email, phone, location, linkedin, portfolio },
//   professionalSummary: string,
//   experience: [{ title, company, startDate, endDate, description }],
//   education: [{ degree, institution, year, grade }],
//   skills: [string],
//   projects: [{ title, description, technologies, link }]
// }
// Response: { success, profile }
```
**Purpose**: Create or update resume profile. This data is used for AI recommendations.

##### `deleteProfile()`
```javascript
// DELETE /api/applicant/profile
// Response: { success, message }
```
**Purpose**: Delete profile permanently.

##### `uploadProfilePicture(file)`
```javascript
// POST /api/applicant/profile/picture
// Request: FormData with image file
// Response: { success, pictureUrl }
```
**Purpose**: Upload profile picture.

##### `downloadCV()`
```javascript
// GET /api/applicant/profile/download-cv
// Response: PDF file
```
**Purpose**: Download generated CV/resume as PDF.

##### `checkProfileCompletion()`
```javascript
// GET /api/applicant/profile/completion
// Response: { success, completionPercentage, missingFields: [] }
```
**Purpose**: Check profile completion status for recommendations readiness.

---

### 4. Company API (`company_api.js`)

**Base URL**: `http://localhost:4000`

##### `getCompanyProfile()`
```javascript
// GET /api/companies/profile
// Response: { success, company }
```
**Purpose**: Get company profile data.

##### `updateCompanyProfile(companyData)`
```javascript
// PUT /api/companies/profile
// Request: { companyName, description, website, location, phoneNo, industry, companySize, logo }
// Response: { success, company }
```
**Purpose**: Update company information.

##### `getCompanyStats()`
```javascript
// GET /api/companies/stats
// Response: { success, stats: { totalJobs, activeJobs, totalApplications, pendingApplications } }
```
**Purpose**: Get dashboard statistics.

##### `getCompanyJobs()`
```javascript
// GET /api/companies/jobs
// Response: { success, jobs: [] }
```
**Purpose**: Get all jobs posted by company (same as job_api).

---

### 5. Recommendation API (`recommendation_api.js`)

**Base URL**: `http://localhost:8000` (Python FastAPI Server)

This API handles AI-powered job recommendations using machine learning.

##### `getJobRecommendations(applicantId, topK, filters)`
```javascript
// POST /api/recommendations/
// Request: { applicant_id, top_k: 10, filters: { location, job_type } }
// Response: { 
//   success, 
//   recommendations: [{ 
//     job_id, 
//     title, 
//     company, 
//     similarity_score, 
//     match_reasons: [] 
//   }] 
// }
```
**Purpose**: Get AI-powered job recommendations based on profile analysis. Uses vector embeddings and similarity matching.

##### `ingestProfile(profileData)`
```javascript
// POST /api/profiles/ingest
// Request: { ...complete profile data }
// Response: { success, message }
```
**Purpose**: Ingest applicant profile into Pinecone vector database for future recommendations.

##### `updateProfile(profileData)`
```javascript
// PUT /api/profiles/update
// Request: { ...updated profile data }
// Response: { success, message }
```
**Purpose**: Update profile vectors in recommendation system.

##### `deleteProfile(applicantId)`
```javascript
// DELETE /api/profiles/:applicantId
// Response: { success, message }
```
**Purpose**: Remove profile from recommendation database.

#### AI Enhancement Endpoints:

##### `enhanceSummary(summaryText)`
```javascript
// POST /api/ai/enhance-summary
// Request: { summary: string }
// Response: { success, enhancedSummary: string }
```
**Purpose**: Use AI to improve professional summary writing.

##### `enhanceExperience(experienceData)`
```javascript
// POST /api/ai/enhance-experience
// Request: { title, company, description }
// Response: { success, enhancedDescription: string }
```
**Purpose**: Enhance work experience descriptions with AI.

##### `enhanceEducation(educationData)`
```javascript
// POST /api/ai/enhance-education
// Request: { degree, institution, achievements }
// Response: { success, enhancedAchievements: string }
```
**Purpose**: Improve education section content.

##### `enhanceProject(projectData)`
```javascript
// POST /api/ai/enhance-project
// Request: { title, description, technologies }
// Response: { success, enhancedDescription: string }
```
**Purpose**: Enhance project descriptions with AI.

##### `generateJobDescription(jobDetails)`
```javascript
// POST /api/ai/generate-job-description
// Request: { title, requirements: [], skills: [] }
// Response: { success, description: string, responsibilities: [] }
```
**Purpose**: AI-powered job description generation for companies.

---

## 🎯 State Management

### Global State (AppContext)

**File**: `src/context/AppContext.jsx`

The application uses React Context API for global state management.

#### Context Provider Structure:
```javascript
export const AppContext = createContext();

export const AppContextProvider = (props) => {
  // State
  const [loggedInUser, setLoggedInUser] = useState(false);
  const [userData, setUserData] = useState(null);
  const backendURL = process.env.NEXT_PUBLIC_BACKEND_URL;
  const server2URL = process.env.NEXT_PUBLIC_SERVER2_URL;
  
  // Methods
  const getAuthState = () => { ... }
  const getUserData = async () => { ... }
  const logout = () => { ... }
  
  return (
    <AppContext.Provider value={{ ... }}>
      {props.children}
    </AppContext.Provider>
  );
}
```

#### Available Context Values:
- `loggedInUser` (boolean) - Is user logged in?
- `setLoggedInUser` (function) - Update login state
- `userData` (object) - Current user/company data
- `setUserData` (function) - Update user data
- `backendURL` (string) - Main API URL
- `server2URL` (string) - AI API URL
- `getAuthState` (function) - Check authentication
- `getUserData` (function) - Fetch current user data
- `logout` (function) - Logout and clear session

#### Usage Example:
```javascript
import { useContext } from 'react';
import { AppContext } from '@/context/AppContext';

const MyComponent = () => {
  const { loggedInUser, userData, logout } = useContext(AppContext);
  
  return (
    <div>
      {loggedInUser && <p>Welcome, {userData?.name}!</p>}
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

---

## 🔑 Authentication System

### Authentication Flow

#### 1. Login Process:
```
User enters credentials → loginUser() API call → Receives JWT token → 
Save token (localStorage + cookies) → Save user role → Redirect to dashboard
```

#### 2. Token Storage:
- **Cookies**: Access token (7 days), Refresh token (30 days)
- **localStorage**: Token, user role, user ID, user data JSON

#### 3. Protected Routes:
```javascript
// Middleware in layout components checks:
useEffect(() => {
  const token = getAccessToken();
  if (!token) {
    router.push('/signin');
  }
}, []);
```

#### 4. API Request Authentication:
```javascript
// Axios interceptor adds token to all requests
instance.interceptors.request.use((config) => {
  const token = getAccessToken();
  if (token) {
    config.headers['Authorization'] = `Bearer ${token}`;
  }
  return config;
});
```

#### 5. Logout Process:
```
User clicks logout → Clear localStorage → Clear cookies → 
Set loggedInUser = false → Redirect to login page
```

### Token Management

**File**: `src/utils/tokenManager.js`

Functions:
- `saveToken(token)` - Save JWT to localStorage
- `getToken()` - Retrieve token
- `removeToken()` - Clear token
- `isAuthenticated()` - Check if token exists
- `saveUser(user)` - Save user data object
- `getUser()` - Get user data
- `clearAuthData()` - Clear all auth data

### Cookie Management

**File**: `src/utils/cookiesManager.js`

Functions:
- `setCookie(name, value, days)` - Set cookie with expiration
- `getCookie(name)` - Get cookie value
- `deleteCookie(name)` - Delete specific cookie
- `saveAuthData(data)` - Save complete auth data (tokens + user)
- `getAccessToken()` - Get access token from cookies
- `getRefreshToken()` - Get refresh token
- `getUserId()` - Get user ID from cookie
- `getUserRole()` - Get user role
- `clearAuthData()` - Clear all auth cookies

---

## 🧩 Core Components

### Landing Page Components

**Location**: `src/components/landing/`

#### 1. **Navbar** (`layouts/Navbar/Navbar.jsx`)
- Responsive navigation bar
- Links to features, pricing, login/signup
- Sticky on scroll with backdrop blur
- Mobile menu with hamburger icon

#### 2. **Hero** (`Hero/Hero.jsx`)
- Landing page hero section
- Animated text and CTAs
- Background graphics with Three.js
- "Get Started" and "Learn More" buttons

#### 3. **KeyFeatures** (`keyFeatures/keyFeatures.jsx`)
- Showcase key platform features
- Grid layout with icons
- Animated on scroll with Framer Motion

#### 4. **ChooseUs** (`ChooseUs/ChooseUs.jsx`)
- Why choose CareerKonnekt section
- Benefits for applicants and companies
- Feature highlights with icons

#### 5. **RecruitmentTimeline** (`RecruitmentTimeline/RecruitmentTimeLine.jsx`)
- 8-step recruitment workflow visualization
- Carousel/slider components
- Step-by-step process explanation

#### 6. **PricingPlan** (`PricingPlan/PricingPlan.jsx`)
- Pricing tiers for companies
- Free and paid plans
- Feature comparison

#### 7. **FAQ** (`home/FAQ/FAQ.jsx`)
- Frequently asked questions
- Expandable accordion items
- Data from `LandingFAQ.js`

#### 8. **Footer** (`layouts/Footer/Footer.jsx`)
- Site links and information
- Social media links
- Copyright and legal links

#### 9. **SmoothScroll** (`home/SmoothScroll/SmoothScroll.jsx`)
- Wrapper using Lenis for smooth scrolling
- Enhances user experience

### Applicant Components

**Location**: `src/components/main-app/applicant/`

#### ApplicantLayout (`applicantLayout/ApplicantLayout.jsx`)
- Sidebar navigation for applicant
- Links: Dashboard, Jobs, Profile
- User profile dropdown
- Logout button
- Wraps all applicant pages

#### ApplicantDashboard (`applicantDashboard/ApplicantDashboard.jsx`)
- Dashboard overview
- Statistics: Applications, Saved Jobs, Profile Views
- Recent activity
- Recommended jobs widget

#### ApplicantJobs (`ApplicantJobs/ApplicantJobs.jsx`)
- Job listing page
- Search and filter functionality
- Job cards with quick actions
- Pagination

#### Applicant Profile Components

**Location**: `Applicant Profile/`

These components work together to create a comprehensive resume builder:

1. **ProfileHeader** - Display name, contact info
2. **ProgressBar** - Show profile completion percentage
3. **PersonalInfoForm** - Name, email, phone, location, links
4. **ProfessionalSummaryForm** - Career summary with AI enhancement
5. **ExperienceForm** - Work history (add/edit/delete multiple entries)
6. **EducationForm** - Educational background
7. **SkillsForm** - Skills with categories
8. **ProjectsForm** - Portfolio projects
9. **TemplateSelector** - Choose CV template design
10. **ColorSelector** - Customize CV colors
11. **ResumePreview** - Live preview of resume
12. **NavigationButtons** - Previous/Next/Save buttons

**Multi-step Form Flow:**
```
Personal Info → Summary → Experience → Education → 
Skills → Projects → Template Selection → Preview & Download
```

### Company Components

**Location**: `src/components/main-app/company/`

#### CompanyLayout (`companyLayout/CompanyLayout.jsx`)
- Sidebar navigation for company
- Links: Dashboard, Jobs, Post Job, Applicants, Interviews, Settings
- Company profile dropdown
- Logout button

#### CompanyDashboard (`companyDashboard/CompanyDashboard.jsx`)
- Analytics dashboard
- Charts: Applications over time, job performance
- Statistics cards
- Recent applications list

#### CompanyPageHeader (`CompanyPageHeader/CompanyPageHeader.jsx`)
- Page title component
- Breadcrumbs
- Action buttons

#### Company Jobs Components

**Location**: `companyJobs/`

1. **JobStats** - Display job metrics (views, applications)
2. **JobFilterTabs** - Filter: All, Active, Drafts, Closed
3. **EmptyJobsState** - Shown when no jobs exist
4. **JobCard** - Individual job listing card
5. **BasicInformationSection** - Job title, type, location
6. **JobDetailsSection** - Description, requirements
7. **CompensationSection** - Salary, benefits
8. **JobRequirementsSection** - Skills, experience level
9. **JobFormActions** - Save, Publish, Cancel buttons
10. **AIJobModal** - AI-powered job description generator popup

**Job Creation Flow:**
```
Click "Post Job" → Fill Basic Info → Add Details → 
Set Requirements → Add Compensation → 
(Optional: Use AI to enhance) → Preview → Publish
```

### Common Components

**Location**: `src/components/common/`

#### Loader (`Loader/Loader.jsx`)
- Full-screen loading spinner
- Used during API calls
- Animated with CSS

---

## 📄 Pages Overview

### Authentication Pages

**Location**: `src/app/(auth)/`

#### `/signin` - Login Page
- Email + password form
- "Remember me" checkbox
- Forgot password link
- Signup modal for user type selection
- Redirects based on role:
  - Applicant → `/applicant/dashboard`
  - Company → `/company/dashboard`

#### `/signup/user` - Applicant Registration
- Full name, email, phone, location
- Current role (optional)
- Password confirmation
- Email verification required after signup
- Redirects to email verification page

#### `/signup/company` - Company Registration
- Company name, email, phone
- Website, location, industry
- Company size selection
- Password confirmation
- Redirects to company dashboard after verification

#### `/forgot-password` - Password Reset Request
- Email input
- Sends OTP to email
- Redirects to reset password page

#### `/reset-password` - New Password Form
- OTP verification
- New password + confirm
- Strength indicator
- Redirects to login after success

#### `/verify-email` - Email Verification
- OTP input (6 digits)
- Resend OTP button
- Auto-verify on correct OTP
- Redirects to dashboard

### Landing Page

**Location**: `src/app/(landing)/home/`

#### `/home` - Main Landing Page
Sections:
1. Hero - Welcome message and CTAs
2. Key Features - Platform capabilities
3. Why Choose Us - Benefits
4. Recruitment Timeline - Process overview
5. Pricing Plans - Subscription tiers
6. FAQs - Common questions
7. Get Started CTA - Final conversion

### Applicant Pages

**Location**: `src/app/applicant/`

#### `/applicant/dashboard`
- Welcome message
- Quick stats (applications, profile views)
- Recommended jobs (AI-powered)
- Recent activity timeline
- Profile completion prompt

#### `/applicant/jobs`
- Job listings with search bar
- Filters: Location, Job Type, Experience Level
- Sort: Newest, Salary, Relevance
- Job cards with:
  - Company logo
  - Job title and location
  - Salary range
  - Quick apply button
- Pagination

#### `/applicant/jobs/[jobId]`
Dynamic route for job details:
- Full job description
- Requirements and responsibilities
- Company information
- Application form:
  - Cover letter (optional)
  - Attach resume
  - Submit button
- Similar jobs section

#### `/applicant/profile`
Resume builder with:
- Step-by-step form
- AI enhancement buttons
- Live preview
- Save progress functionality
- Download as PDF
- Template selection
- Color customization

### Company Pages

**Location**: `src/app/company/`

#### `/company/dashboard`
- Analytics dashboard
- Charts and graphs
- Quick stats:
  - Total jobs posted
  - Active applications
  - Profile views
- Recent applications table
- Quick actions

#### `/company/jobs`
- List all posted jobs
- Filter tabs: All, Active, Drafts, Closed
- Job cards with:
  - Job title and stats
  - Edit/Delete buttons
  - View applications
- Sort options

#### `/company/post-job`
Multi-section form:
1. Basic Information (title, type, location)
2. Job Details (description, responsibilities)
3. Requirements (skills, experience, education)
4. Compensation (salary range, benefits)
5. AI Enhancement (optional)
6. Review & Publish

Features:
- Save as draft
- AI description generator
- Preview mode
- Publish to go live

#### `/company/applicants`
- View all applications
- Filter by job, status
- Sort by date, relevance
- Applicant cards with:
  - Name and photo
  - Applied position
  - Status (pending, shortlisted, rejected)
  - View profile button
  - Change status dropdown

#### `/company/interviews`
- Schedule interviews
- Calendar view
- Interview list
- Create interview:
  - Select applicant
  - Choose date/time
  - Set interview type (phone, video, in-person)
  - Add notes

#### `/company/settings`
- Company profile edit
- Change password
- Notification preferences
- Billing information
- Delete account

### Other Pages

#### `/cookies` - Cookie Policy
- Information about cookie usage
- User consent
- Privacy details

---

## 🔧 Utilities

### Validation Utilities (`utils/validation.js`)

Comprehensive form validation functions:

#### `validateEmail(email)`
- Checks email format using regex
- Returns `{ valid: boolean, message?: string }`

#### `validatePhone(phone)`
- Validates phone number format
- Optional field

#### `validateURL(url)`
- Checks valid URL format
- Used for website, portfolio links

#### `validateLinkedIn(url)`
- Specific validation for LinkedIn URLs
- Ensures proper format

#### `validateRequired(value, fieldName)`
- Checks if field is not empty
- Generic required field validator

#### `validateLength(value, min, max, fieldName)`
- Checks string length constraints
- Used for passwords, descriptions

#### `validateDateFormat(date)`
- Validates YYYY-MM format
- Used for experience dates

#### `validatePersonalInfo(data)`
- Validates entire personal info section
- Returns array of errors

#### `validateExperience(data)`
- Validates work experience entry
- Checks required fields

#### `validateEducation(data)`
- Validates education entry

#### `validateProject(data)`
- Validates project entry

#### `validateJobPost(jobData)`
- Validates complete job posting
- Ensures all required fields

#### `validatePassword(password)`
- Password strength validation
- Checks length, characters, special chars
- Returns strength level

### Form Validation Usage:
```javascript
import { validateEmail, validateRequired } from '@/utils/validation';

const handleSubmit = () => {
  const emailValidation = validateEmail(formData.email);
  if (!emailValidation.valid) {
    toast.error(emailValidation.message);
    return;
  }
  // Continue processing...
}
```

---

## 🎨 Styling & Theming

### Global Styles (`globals.css`)

**Custom CSS Features:**

1. **Smooth Animations:**
```css
@keyframes fadeInUp { ... }
@keyframes scaleIn { ... }
@keyframes slideDown { ... }
@keyframes float { ... }
@keyframes drift { ... }
```

2. **Utility Classes:**
- `.scrollbar-hide` - Hide scrollbars
- `.animate-float` - Floating animation
- `.animate-drift` - Drifting motion
- `.animate-scale-in` - Scale in animation

3. **Interactive Elements:**
- Smooth transitions on buttons
- Hover effects
- Active states
- Disabled states
- Cursor styles

4. **Custom Breakpoints:**
```css
@custom-media --xs (min-width: 475px);
```

### Tailwind Configuration

**Font**: Poppins (weights: 300, 400, 500, 600, 700, 800)

**Color Palette:**
- Primary: `#091e40` (Dark blue)
- White: `#ffffff`
- Gray shades for text and backgrounds
- Success, error, warning states via Tailwind defaults

**Responsive Breakpoints:**
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

---

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ (recommended: v20)
- npm or yarn
- Git

### Installation

1. **Clone the repository:**
```bash
git clone <repository-url>
cd Code/Frontend/CareerKonnekt-Frontend
```

2. **Install dependencies:**
```bash
npm install
```

3. **Create environment file:**
```bash
# Copy .env.local.example to .env.local
cp .env.local.example .env.local

# Edit with your API URLs
NEXT_PUBLIC_BACKEND_URL="http://localhost:4000"
NEXT_PUBLIC_API_URL="http://localhost:4000"
NEXT_PUBLIC_AI_BACKEND_URL="http://localhost:8001"
```

4. **Run development server:**
```bash
npm run dev
```

5. **Open browser:**
```
http://localhost:3000
```

### Available Scripts

**Development:**
```bash
npm run dev        # Start Next.js dev server (hot reload)
```

**Production:**
```bash
npm run build      # Build optimized production bundle
npm run start      # Start production server
```

**Code Quality:**
```bash
npm run lint       # Run ESLint to check code
```

---

## 🛠 Development Guide

### Project Structure Guidelines

1. **Pages** go in `src/app/` (App Router)
2. **Components** go in `src/components/`
   - Reusable → `common/`
   - Feature-specific → `main-app/[feature]/`
   - Landing → `landing/`
3. **API calls** go in `src/services/api/`
4. **Utilities** go in `src/utils/`
5. **Static assets** go in `public/` or `src/assets/`

### Coding Conventions

#### File Naming:
- **Components**: PascalCase (e.g., `UserProfile.jsx`)
- **Pages**: lowercase (e.g., `page.jsx`, `layout.jsx`)
- **Utilities**: camelCase (e.g., `validation.js`)
- **API services**: snake_case folders, camelCase files (e.g., `auth_api/auth_api.js`)

#### Component Structure:
```javascript
'use client'; // If using hooks or client-side features

import React, { useState } from 'react';
import { useRouter } from 'next/navigation';

const ComponentName = () => {
  // State
  const [data, setData] = useState(null);
  
  // Hooks
  const router = useRouter();
  
  // Effects
  useEffect(() => {
    fetchData();
  }, []);
  
  // Handlers
  const handleAction = () => {
    // Logic
  };
  
  // Render
  return (
    <div>
      {/* JSX */}
    </div>
  );
};

export default ComponentName;
```

### API Integration Pattern

```javascript
// In component
import { apiFunction } from '@/services/api/module_api/module_api';
import { toast } from 'react-toastify';

const handleAction = async () => {
  try {
    const response = await apiFunction(data);
    if (response.success) {
      toast.success('Success!');
      // Update UI
    } else {
      toast.error(response.message);
    }
  } catch (error) {
    console.error(error);
    toast.error(error.message || 'An error occurred');
  }
};
```

### State Management Pattern

**For global state**, use Context:
```javascript
const { loggedInUser, userData, logout } = useContext(AppContext);
```

**For local state**, use useState:
```javascript
const [formData, setFormData] = useState({});
```

**For side effects**, use useEffect:
```javascript
useEffect(() => {
  // Fetch data on mount
  fetchData();
}, []); // Empty array = run once
```

### Routing Pattern

**Navigation:**
```javascript
import { useRouter } from 'next/navigation';

const Component = () => {
  const router = useRouter();
  
  const handleClick = () => {
    router.push('/applicant/dashboard');
  };
};
```

**Links:**
```javascript
import Link from 'next/link';

<Link href="/about">About</Link>
```

**Dynamic routes:**
```javascript
// File: app/applicant/jobs/[jobId]/page.jsx
const JobDetails = ({ params }) => {
  const { jobId } = params;
  // Use jobId
};
```

### Error Handling Pattern

```javascript
try {
  const response = await apiCall();
  
  if (response.success) {
    // Handle success
  } else {
    // Handle API error response
    toast.error(response.message);
  }
} catch (error) {
  // Handle network/system error
  console.error('Error:', error);
  toast.error(error.message || 'Something went wrong');
}
```

### Form Handling Pattern

```javascript
const [formData, setFormData] = useState({
  email: '',
  password: ''
});

const handleChange = (e) => {
  setFormData({
    ...formData,
    [e.target.name]: e.target.value
  });
};

const handleSubmit = async (e) => {
  e.preventDefault();
  
  // Validate
  const validation = validateForm(formData);
  if (!validation.valid) {
    toast.error(validation.message);
    return;
  }
  
  // Submit
  await submitData(formData);
};

return (
  <form onSubmit={handleSubmit}>
    <input
      name="email"
      value={formData.email}
      onChange={handleChange}
    />
    <button type="submit">Submit</button>
  </form>
);
```

---

## 📦 Dependencies Explanation

### Production Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `next` | 16.1.6 | React framework with SSR, routing |
| `react` | 19.2.3 | UI library |
| `react-dom` | 19.2.3 | React rendering for web |
| `axios` | 1.13.4 | HTTP client for API calls |
| `framer-motion` | 12.30.0 | Animation library |
| `lenis` | 1.3.17 | Smooth scrolling |
| `three` | 0.182.0 | 3D graphics |
| `lucide-react` | 0.563.0 | Icon set |
| `react-toastify` | 11.0.5 | Toast notifications |
| `sweetalert2` | 11.26.18 | Alert modals |
| `html2canvas` | 1.4.1 | HTML to image conversion |
| `jspdf` | 4.1.0 | PDF generation |
| `maath` | 0.10.8 | Math utilities for 3D |

### Development Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `tailwindcss` | 4 | CSS framework |
| `@tailwindcss/postcss` | 4 | PostCSS plugin |
| `eslint` | 9 | Code linting |
| `eslint-config-next` | 16.1.6 | ESLint config for Next.js |
| `babel-plugin-react-compiler` | 1.0.0 | React optimizations |

---

## 🔒 Security Considerations

### Authentication Security
1. **JWT Tokens**: Secure token storage in cookies with HttpOnly flag (recommended for production)
2. **Token Refresh**: Refresh token mechanism (30-day expiry)
3. **Protected Routes**: Client-side route guards
4. **Authorization Headers**: Bearer token in all authenticated requests

### Data Protection
1. **HTTPS Only**: Use HTTPS in production
2. **Input Validation**: Both frontend and backend validation
3. **XSS Prevention**: React escapes user inputs by default
4. **CSRF Protection**: SameSite cookie attribute

### Best Practices Implemented
1. Password strength validation
2. Email verification before account activation
3. OTP-based password reset
4. Session timeout handling
5. Secure cookie configuration

---

## 🐛 Common Issues & Solutions

### Issue: "Module not found" error
**Solution**: 
```bash
npm install
# or
rm -rf node_modules package-lock.json
npm install
```

### Issue: Environment variables not working
**Solution**: 
- Ensure `.env.local` exists
- Restart dev server after changing env vars
- Prefix browser-accessible vars with `NEXT_PUBLIC_`

### Issue: API calls failing
**Solution**:
1. Check backend server is running (Port 4000)
2. Check AI server is running (Port 8000/8001)
3. Verify environment variables
4. Check network tab in browser DevTools

### Issue: Authentication not persisting
**Solution**:
1. Check localStorage and cookies in browser
2. Ensure token is saved after login
3. Verify token expiry date
4. Check axios interceptor is adding token

### Issue: Build errors
**Solution**:
```bash
# Clear Next.js cache
rm -rf .next

# Rebuild
npm run build
```

---

## 📚 Additional Resources

### Next.js Documentation
- [Next.js Docs](https://nextjs.org/docs)
- [App Router](https://nextjs.org/docs/app)
- [Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching)

### React Resources
- [React Docs](https://react.dev/)
- [React Hooks](https://react.dev/reference/react)

### Styling
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Framer Motion](https://www.framer.com/motion/)

### State Management
- [React Context](https://react.dev/reference/react/useContext)

---

## 🤝 Contributing Guidelines

### Git Workflow
1. Create feature branch: `git checkout -b feature/feature-name`
2. Make changes and commit: `git commit -m "feat: description"`
3. Push to branch: `git push origin feature/feature-name`
4. Create Pull Request

### Commit Message Format
```
feat: Add new feature
fix: Fix bug
docs: Update documentation
style: Format code
refactor: Refactor code
test: Add tests
chore: Update dependencies
```

---

## 📞 Support & Contact

For issues and questions:
- Check existing documentation
- Review activity diagrams in `docs/activity-diagrams/`
- Review sequence diagrams in `docs/sequence-diagrams/`
- Check ERD in `docs/erd/`

---

## 📄 License

[Add your license information here]

---

## 🎉 Conclusion

This frontend application is built with modern web technologies and follows best practices for:
- **Performance**: Code splitting, lazy loading, optimized builds
- **User Experience**: Smooth animations, responsive design, intuitive UI
- **Scalability**: Modular architecture, reusable components
- **Maintainability**: Clear structure, documented code, consistent patterns
- **Security**: Secure authentication, input validation, protected routes

The application seamlessly integrates with two backend services to provide a complete AI-powered recruitment platform for both job seekers and employers.

---

**Last Updated**: February 11, 2026
**Version**: 1.0.0
**Maintainer**: CareerKonnekt Team
