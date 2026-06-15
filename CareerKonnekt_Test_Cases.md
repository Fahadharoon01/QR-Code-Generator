# CareerKonnekt - Test Cases

---

## 1. User Registration & Authentication

### 1.1 Applicant Registration

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REG-01 |
| Component Name | Applicant Registration |
| Description | Verify applicant can register successfully |
| User | Applicant |
| Pre-Requisite | Applicant is not already registered |
| Steps Performed | • Open Register Page • Enter valid details • Click Register • Verify OTP |
| Input | Valid name, email, password |
| Expected Result | Account created, verification OTP sent, account verified after OTP |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REG-02 |
| Component Name | Invalid Email Registration |
| Description | Verify registration fails with invalid email format |
| User | Applicant |
| Pre-Requisite | N/A |
| Steps Performed | • Open Register Page • Enter invalid email • Click Register |
| Input | Invalid email (e.g., "test.com") |
| Expected Result | Validation error shown, registration not processed |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REG-03 |
| Component Name | OTP Verification - Correct |
| Description | Verify correct OTP verifies account |
| User | Applicant |
| Pre-Requisite | Account registered, OTP sent |
| Steps Performed | • Enter correct OTP • Click Verify |
| Input | 6-digit OTP |
| Expected Result | Account verified, redirect to profile wizard |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REG-04 |
| Component Name | OTP Verification - Incorrect |
| Description | Verify wrong OTP shows error |
| User | Applicant |
| Pre-Requisite | Account registered, OTP sent |
| Steps Performed | • Enter wrong OTP • Click Verify |
| Input | Wrong 6-digit code |
| Expected Result | Error message, retry allowed |
| Test Result | |

---

### 1.2 Company Registration

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REG-05 |
| Component Name | Company Registration |
| Description | Verify company can register successfully |
| User | Company/HR |
| Pre-Requisite | Company not already registered |
| Steps Performed | • Open Register Page • Select "Company" • Enter valid details • Verify OTP |
| Input | Company name, valid email, password |
| Expected Result | Company account created and verified |
| Test Result | |

---

### 1.3 Login

| Field | Description |
|-------|-------------|
| Test Case ID | TC-LOG-01 |
| Component Name | Applicant Login |
| Description | Verify applicant can login with correct credentials |
| User | Applicant |
| Pre-Requisite | Account registered and verified |
| Steps Performed | • Open Login Page • Enter email & password • Click Login |
| Input | Correct email & password |
| Expected Result | Redirect to applicant dashboard |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-LOG-02 |
| Component Name | Invalid Login |
| Description | Verify login fails with wrong password |
| User | Applicant/Company |
| Pre-Requisite | Account exists |
| Steps Performed | • Enter correct email • Enter wrong password • Click Login |
| Input | Wrong password |
| Expected Result | Error message, not logged in |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-LOG-03 |
| Component Name | Company Login |
| Description | Verify company can login successfully |
| User | Company/HR |
| Pre-Requisite | Company account exists |
| Steps Performed | • Login with company credentials |
| Input | Company email & password |
| Expected Result | Redirect to company dashboard |
| Test Result | |

---

## 2. Applicant Profile Management

### 2.1 Profile Creation

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PROF-01 |
| Component Name | Profile Wizard Completion |
| Description | Verify applicant can complete profile wizard |
| User | Applicant |
| Pre-Requisite | Logged in, profile not complete |
| Steps Performed | • Complete all 7 steps • Save profile |
| Input | Personal info, education, experience, skills, projects |
| Expected Result | Profile saved, marked as complete |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PROF-02 |
| Component Name | Profile Photo Upload |
| Description | Verify profile photo can be uploaded |
| User | Applicant |
| Pre-Requisite | Logged in |
| Steps Performed | • Go to profile • Select photo • Upload |
| Input | Valid image file (JPG/PNG) |
| Expected Result | Photo uploaded, displayed in profile |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PROF-03 |
| Component Name | AI Background Removal |
| Description | Verify AI background removal works |
| User | Applicant |
| Pre-Requisite | Photo uploaded |
| Steps Performed | • Click "Remove Background" |
| Input | N/A (uses uploaded photo) |
| Expected Result | Photo background removed, processed image shown |
| Test Result | |

---

### 2.2 Resume Upload & Parsing

| Field | Description |
|-------|-------------|
| Test Case ID | TC-RES-01 |
| Component Name | Resume Upload |
| Description | Verify resume can be uploaded |
| User | Applicant |
| Pre-Requisite | Logged in |
| Steps Performed | • Select resume file • Upload |
| Input | PDF/DOCX file |
| Expected Result | Resume uploaded to Cloudinary |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-RES-02 |
| Component Name | AI Resume Parsing |
| Description | Verify AI parses resume correctly |
| User | Applicant |
| Pre-Requisite | Resume uploaded |
| Steps Performed | • Click "Parse Resume" |
| Input | Uploaded resume |
| Expected Result | Resume parsed, profile fields auto-filled |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-RES-03 |
| Component Name | Invalid Resume File |
| Description | Verify invalid file type rejected |
| User | Applicant |
| Pre-Requisite | Logged in |
| Steps Performed | • Try to upload non-PDF/DOCX file |
| Input | TXT/EXE/etc. file |
| Expected Result | Error: "Please upload PDF or DOCX" |
| Test Result | |

---

### 2.3 AI Profile Enhancement

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PROF-04 |
| Component Name | AI Profile Enhancement |
| Description | Verify AI improves profile content |
| User | Applicant |
| Pre-Requisite | Profile has content |
| Steps Performed | • Click "AI Enhance" • Review changes • Save |
| Input | Existing profile content |
| Expected Result | Profile content improved with AI |
| Test Result | |

---

### 2.4 CV Download

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PROF-05 |
| Component Name | CV Generation & Download |
| Description | Verify CV can be downloaded with selected template |
| User | Applicant |
| Pre-Requisite | Profile complete |
| Steps Performed | • Select template • Click "Download CV" |
| Input | Template selection |
| Expected Result | PDF CV downloaded with correct content |
| Test Result | |

---

## 3. Job Browse & Application

### 3.1 Job Browse & Search

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-01 |
| Component Name | View Active Jobs |
| Description | Verify active jobs are displayed |
| User | Applicant |
| Pre-Requisite | Logged in |
| Steps Performed | • Go to Jobs page |
| Input | N/A |
| Expected Result | All active jobs shown |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-02 |
| Component Name | View Job Details |
| Description | Verify job details page loads correctly |
| User | Applicant |
| Pre-Requisite | Jobs available |
| Steps Performed | • Click on a job |
| Input | N/A |
| Expected Result | Full job details displayed |
| Test Result | |

---

### 3.2 Job Application

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-03 |
| Component Name | Apply for Job |
| Description | Verify applicant can apply for a job |
| User | Applicant |
| Pre-Requisite | Logged in, profile complete, not applied before |
| Steps Performed | • Go to job • Click "Apply Now" • Submit |
| Input | Application data |
| Expected Result | Application submitted, status updated |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-04 |
| Component Name | Re-apply for Same Job |
| Description | Verify cannot apply for same job twice |
| User | Applicant |
| Pre-Requisite | Already applied for a job |
| Steps Performed | • Try to apply again |
| Input | N/A |
| Expected Result | Error: "You already applied for this job" |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-05 |
| Component Name | View My Applications |
| Description | Verify applicant can see their applications |
| User | Applicant |
| Pre-Requisite | Applied for some jobs |
| Steps Performed | • Go to "My Applications" |
| Input | N/A |
| Expected Result | List of all applications with status |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JOB-06 |
| Component Name | Withdraw Application |
| Description | Verify applicant can withdraw application |
| User | Applicant |
| Pre-Requisite | Applied for a job |
| Steps Performed | • Click "Withdraw" • Confirm |
| Input | N/A |
| Expected Result | Application status changed to withdrawn |
| Test Result | |

---

## 4. AI Interviews

### 4.1 Start AI Interview

| Field | Description |
|-------|-------------|
| Test Case ID | TC-AI-01 |
| Component Name | Join AI Interview |
| Description | Verify can join AI interview with sufficient tokens |
| User | Applicant |
| Pre-Requisite | Invited for AI interview, has tokens |
| Steps Performed | • Open interview link • Click "Join" |
| Input | N/A |
| Expected Result | AI interview starts (Sarah Mitchell persona) |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-AI-02 |
| Component Name | Insufficient Tokens |
| Description | Verify can't join without enough tokens |
| User | Applicant |
| Pre-Requisite | Invited, but 0 tokens |
| Steps Performed | • Try to join interview |
| Input | N/A |
| Expected Result | Error: "Upgrade to Pro required" |
| Test Result | |

---

### 4.2 AI Interview Flow

| Field | Description |
|-------|-------------|
| Test Case ID | TC-AI-03 |
| Component Name | AI Question Flow |
| Description | Verify AI asks 5 questions in order |
| User | Applicant |
| Pre-Requisite | In AI interview |
| Steps Performed | • Complete all 5 questions |
| Input | Answers (text/audio) |
| Expected Result | All 5 questions asked, then review phase |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-AI-04 |
| Component Name | AI Interview Evaluation |
| Description | Verify AI generates score and feedback |
| User | Applicant |
| Pre-Requisite | Completed AI interview |
| Steps Performed | • Finish interview • View results |
| Input | N/A |
| Expected Result | Score, confidence, strengths/weaknesses shown |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-AI-05 |
| Component Name | Token Deduction |
| Description | Verify tokens deducted correctly |
| User | Applicant |
| Pre-Requisite | Completed AI interview |
| Steps Performed | • Check token balance after interview |
| Input | N/A |
| Expected Result | Tokens deducted based on duration |
| Test Result | |

---

## 5. HR Video Interviews (LiveKit)

### 5.1 Schedule Interview

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-01 |
| Component Name | Schedule HR Interview |
| Description | Verify HR can schedule interview |
| User | Company/HR |
| Pre-Requisite | Has applicants, logged in |
| Steps Performed | • Select applicant • Set date/time • Send invite |
| Input | Interview date/time |
| Expected Result | Interview scheduled, email/notification sent |
| Test Result | |

---

### 5.2 Join LiveKit Room

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-02 |
| Component Name | HR Joins Interview |
| Description | Verify HR can join LiveKit room |
| User | Company/HR |
| Pre-Requisite | Interview scheduled |
| Steps Performed | • Click "Join Interview" • Allow camera/mic |
| Input | N/A |
| Expected Result | LiveKit room connected, video/audio working |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-03 |
| Component Name | Applicant Joins Interview |
| Description | Verify applicant can join LiveKit room |
| User | Applicant |
| Pre-Requisite | Interview scheduled, invited |
| Steps Performed | • Open interview link • Click "Join" |
| Input | N/A |
| Expected Result | Applicant connects, both parties can see each other |
| Test Result | |

---

### 5.3 Interview Decision

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-04 |
| Component Name | Hire Applicant |
| Description | Verify HR can mark applicant as hired |
| User | Company/HR |
| Pre-Requisite | Interview completed |
| Steps Performed | • Click "Hire" • Confirm |
| Input | N/A |
| Expected Result | Status updated to hired, offer email sent |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-05 |
| Component Name | Reject Applicant |
| Description | Verify HR can reject applicant |
| User | Company/HR |
| Pre-Requisite | Interview completed |
| Steps Performed | • Click "Reject" • Confirm |
| Input | N/A |
| Expected Result | Status updated to rejected, email sent |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-HR-06 |
| Component Name | Reschedule Interview |
| Description | Verify interview can be rescheduled |
| User | Company/HR |
| Pre-Requisite | Interview scheduled but not completed |
| Steps Performed | • Click "Reschedule" • Pick new time • Save |
| Input | New date/time |
| Expected Result | Interview rescheduled, notification sent |
| Test Result | |

---

## 6. Company Job Posting

### 6.1 Create Job Post

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JPOST-01 |
| Component Name | Create Job Post |
| Description | Verify HR can create new job |
| User | Company/HR |
| Pre-Requisite | Logged in |
| Steps Performed | • Click "Post New Job" • Fill details • Save |
| Input | Job title, description, skills, etc. |
| Expected Result | Job post saved (draft or active) |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JPOST-02 |
| Component Name | AI Job Description |
| Description | Verify AI generates job description |
| User | Company/HR |
| Pre-Requisite | Creating new job |
| Steps Performed | • Click "AI Generate Description" • Review • Save |
| Input | Basic job details |
| Expected Result | AI generates professional JD |
| Test Result | |

---

### 6.2 Publish Job

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JPOST-03 |
| Component Name | Publish Job Post |
| Description | Verify job can be published |
| User | Company/HR |
| Pre-Requisite | Job created as draft |
| Steps Performed | • Click "Publish" |
| Input | N/A |
| Expected Result | Job active, visible to applicants, ingested to Pinecone |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JPOST-04 |
| Component Name | Edit Job Post |
| Description | Verify job can be edited |
| User | Company/HR |
| Pre-Requisite | Job posted |
| Steps Performed | • Click "Edit" • Make changes • Save |
| Input | Updated job details |
| Expected Result | Job updated, re-ingested to Pinecone |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-JPOST-05 |
| Component Name | Close Job Post |
| Description | Verify job can be closed |
| User | Company/HR |
| Pre-Requisite | Job active |
| Steps Performed | • Click "Close Job" |
| Input | N/A |
| Expected Result | Job status = closed, no longer visible |
| Test Result | |

---

## 7. AI Recommendations

### 7.1 Job Recommendations (Applicant)

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REJ-01 |
| Component Name | Show Job Recommendations |
| Description | Verify personalized jobs shown on dashboard |
| User | Applicant |
| Pre-Requisite | Profile complete |
| Steps Performed | • Open dashboard |
| Input | N/A |
| Expected Result | All recommended jobs with match reasons shown |
| Test Result | |

---

### 7.2 Applicant Recommendations (Company)

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REA-01 |
| Component Name | Show Applicant Recommendations |
| Description | Verify AI recommends applicants for a job |
| User | Company/HR |
| Pre-Requisite | Job posted, has profiles in system |
| Steps Performed | • Go to job • Click "AI Recommendations" |
| Input | N/A |
| Expected Result | Qualified applicants (score ≥ 0.65) with matched skills shown |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-REA-02 |
| Component Name | Invite Recommended Applicant |
| Description | Verify can invite recommended applicant |
| User | Company/HR |
| Pre-Requisite | On recommendations page |
| Steps Performed | • Select applicant • Click "Invite to Interview" |
| Input | N/A |
| Expected Result | Interview created, notification sent |
| Test Result | |

---

## 8. Career Guidance Chat

### 8.1 Chat with AI Coach

| Field | Description |
|-------|-------------|
| Test Case ID | TC-CG-01 |
| Component Name | Start Chat Session |
| Description | Verify can chat with Alex Carter AI coach |
| User | Applicant |
| Pre-Requisite | Logged in |
| Steps Performed | • Open Career Guidance • Type question • Send |
| Input | Question (e.g., "How to improve my resume?") |
| Expected Result | AI responds as Alex Carter, chat saved |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-CG-02 |
| Component Name | Chat History Load |
| Description | Verify chat history loads on next visit |
| User | Applicant |
| Pre-Requisite | Had chat before |
| Steps Performed | • Reopen Career Guidance |
| Input | N/A |
| Expected Result | Previous chat messages shown |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-CG-03 |
| Component Name | Multiple Chat Questions |
| Description | Verify can have continuous conversation |
| User | Applicant |
| Pre-Requisite | In chat session |
| Steps Performed | • Ask 3+ questions in a row |
| Input | Multiple questions |
| Expected Result | AI responds to each question |
| Test Result | |

---

## 9. Payment & Subscription

### 9.1 Pro Plan Subscription

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PAY-01 |
| Component Name | Start Pro Subscription |
| Description | Verify can go to Stripe checkout |
| User | Company/HR |
| Pre-Requisite | Logged in as company, not Pro |
| Steps Performed | • Click "Upgrade to Pro" • View plan • Click "Subscribe" |
| Input | N/A |
| Expected Result | Redirect to Stripe checkout |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PAY-02 |
| Component Name | Complete Stripe Payment |
| Description | Verify payment completes successfully |
| User | Company/HR |
| Pre-Requisite | On Stripe checkout |
| Steps Performed | • Enter payment details • Complete payment |
| Input | Credit/debit card info |
| Expected Result | Payment successful, redirect back to app |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PAY-03 |
| Component Name | Webhook Processing |
| Description | Verify webhook updates account to Pro |
| User | N/A (System) |
| Pre-Requisite | Payment completed on Stripe |
| Steps Performed | • Wait for webhook • Check company status |
| Input | N/A |
| Expected Result | `isPro = true`, 20,000 tokens added, transaction recorded |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PAY-04 |
| Component Name | Pro Badge Display |
| Description | Verify Pro badge shown after subscription |
| User | Company/HR |
| Pre-Requisite | Payment completed, webhook processed |
| Steps Performed | • Refresh dashboard |
| Input | N/A |
| Expected Result | Pro badge visible, token balance shows 20,000+ |
| Test Result | |

---

### 9.2 Token Usage

| Field | Description |
|-------|-------------|
| Test Case ID | TC-PAY-05 |
| Component Name | Token Deduction on AI Interview |
| Description | Verify tokens deducted correctly during AI interview |
| User | Applicant |
| Pre-Requisite | Pro company invited them for AI interview |
| Steps Performed | • Do 1 minute AI interview • Check balance |
| Input | N/A |
| Expected Result | 40 tokens deducted (Pro rate) |
| Test Result | |

---

## 10. Notification System

### 10.1 Notification Creation

| Field | Description |
|-------|-------------|
| Test Case ID | TC-NOT-01 |
| Component Name | Interview Invite Notification |
| Description | Verify notification created on interview invite |
| User | N/A (System) |
| Pre-Requisite | HR scheduled interview |
| Steps Performed | • Schedule interview • Check notifications |
| Input | N/A |
| Expected Result | In-app notification + email sent to applicant |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-NOT-02 |
| Component Name | Interview Result Notification |
| Description | Verify notification sent on hire/reject |
| User | N/A (System) |
| Pre-Requisite | HR made decision |
| Steps Performed | • Hire/reject applicant • Check notifications |
| Input | N/A |
| Expected Result | In-app + email sent with decision |
| Test Result | |

---

### 10.2 Notification Display

| Field | Description |
|-------|-------------|
| Test Case ID | TC-NOT-03 |
| Component Name | Bell Badge & Unread Count |
| Description | Verify unread badge shown |
| User | Applicant/Company |
| Pre-Requisite | Has unread notifications |
| Steps Performed | • Open app |
| Input | N/A |
| Expected Result | Bell badge with unread count visible |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-NOT-04 |
| Component Name | Mark Notification as Read |
| Description | Verify notification marked read when viewed |
| User | Applicant/Company |
| Pre-Requisite | Has unread notification |
| Steps Performed | • Click notification • Read it |
| Input | N/A |
| Expected Result | Notification marked read, badge cleared |
| Test Result | |

---

## 11. Integration Tests (External Services)

| Field | Description |
|-------|-------------|
| Test Case ID | TC-INT-01 |
| Component Name | MongoDB Connection |
| Description | Verify app can connect to MongoDB |
| User | N/A (System) |
| Pre-Requisite | Server running |
| Steps Performed | • Check server logs • Try any DB operation |
| Input | N/A |
| Expected Result | Connection successful, DB operations work |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-INT-02 |
| Component Name | Pinecone Vector DB |
| Description | Verify job/profile ingest works |
| User | N/A (System) |
| Pre-Requisite | Job posted / profile created |
| Steps Performed | • Ingest to Pinecone • Check if exists |
| Input | N/A |
| Expected Result | Vectors upserted, queryable |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-INT-03 |
| Component Name | OpenAI API |
| Description | Verify OpenAI calls work |
| User | N/A (System) |
| Pre-Requisite | Any AI feature used |
| Steps Performed | • Use AI interview / enhancement / parsing |
| Input | N/A |
| Expected Result | OpenAI responds without error |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-INT-04 |
| Component Name | Cloudinary Upload |
| Description | Verify file upload to Cloudinary works |
| User | Applicant |
| Pre-Requisite | Uploading photo/resume |
| Steps Performed | • Upload file • Check URL |
| Input | Photo/resume file |
| Expected Result | File uploaded, public URL returned |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-INT-05 |
| Component Name | SMTP Email Sending |
| Description | Verify emails are sent successfully |
| User | N/A (System) |
| Pre-Requisite | Event that triggers email |
| Steps Performed | • Trigger email • Check inbox |
| Input | N/A |
| Expected Result | Email received in inbox |
| Test Result | |

---

## 12. Edge Cases & Error Handling

| Field | Description |
|-------|-------------|
| Test Case ID | TC-EDGE-01 |
| Component Name | Network Disconnect During Interview |
| Description | Verify app handles network loss |
| User | Applicant/HR |
| Pre-Requisite | In LiveKit interview |
| Steps Performed | • Disconnect internet • Reconnect |
| Input | N/A |
| Expected Result | Shows error, tries to reconnect |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-EDGE-02 |
| Component Name | File Too Large Upload |
| Description | Verify oversized files rejected |
| User | Applicant/Company |
| Pre-Requisite | Uploading file |
| Steps Performed | • Try to upload >10MB file |
| Input | Oversized file |
| Expected Result | Error: "File too large" |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-EDGE-03 |
| Component Name | Concurrent LiveKit Room |
| Description | Verify multiple rooms work |
| User | HR + 2 Applicants |
| Pre-Requisite | 2 interviews scheduled at same time |
| Steps Performed | • Join both rooms |
| Input | N/A |
| Expected Result | Both rooms connect independently |
| Test Result | |

---

## 13. Cron Jobs

| Field | Description |
|-------|-------------|
| Test Case ID | TC-CRON-01 |
| Component Name | Expire AI Interviews |
| Description | Verify pending interviews expire after time |
| User | N/A (System) |
| Pre-Requisite | Pending interview created >1hr ago |
| Steps Performed | • Wait for hourly cron • Check interview status |
| Input | N/A |
| Expected Result | Status changed to expired |
| Test Result | |

---

| Field | Description |
|-------|-------------|
| Test Case ID | TC-CRON-02 |
| Component Name | HR No-Show Detection |
| Description | Verify no-show detected after grace period |
| User | N/A (System) |
| Pre-Requisite | Interview started, HR no-show >15min |
| Steps Performed | • Wait for cron • Check status |
| Input | N/A |
| Expected Result | No-show recorded |
| Test Result | |

---

