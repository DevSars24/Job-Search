

# # 💼 Job Portal – MERN + Supabase + Clerk (Full Stack Project)

A modern job portal platform where **Candidates can search/apply for jobs** and **Recruiters can post/manage jobs**.
This project uses:

* **React + Vite** for Frontend
* **Clerk** for Authentication
* **Supabase** for Database, Storage & RLS
* **ShadCN UI + TailwindCSS** for UI
* **Zod + React Hook Form** for Forms & Validation

This is a production-ready architecture inspired by RoadsideCoder’s Job Portal.

---

## 🚀 Live Features At a Glance

### 👨‍💼 **Candidate Features**

* Browse latest jobs
* Search by job title
* Filter by **location**
* Filter by **company**
* Save/Unsave jobs
* Apply to job with **PDF Resume upload**
* Track all applications in **My Applications**
* View job details & company info

---

### 🧑‍💻 **Recruiter Features**

* Onboarding flow with role selection
* Add new companies with **logo upload**
* Post new jobs
* Edit job hiring status (Open / Closed)
* View all applications for a job
* Update application status (Applied → Interviewing → Hired / Rejected)
* Delete job postings
* Manage all posted jobs in **My Jobs**

---

## 🏗️ **Tech Stack**

### **Frontend**

* React + Vite
* React Router
* ShadCN UI
* TailwindCSS
* React Hook Form
* Zod Validation

### **Backend**

* Supabase
* Postgres
* Supabase Storage
* Row Level Security
* Clerk Authentication

---

## 📦 **Folder Structure**

```
src/
 ├── api/               # All API calls to Supabase
 │    ├── apiJobs.js
 │    ├── apiCompanies.js
 │    └── apiApplication.js
 ├── components/        # UI + Business components
 ├── hooks/
 │    └── use-fetch.js  # Custom hook for API abstraction
 ├── layouts/
 ├── pages/             # All page routes (Landing, Jobs, Post Job ...)
 ├── utils/
 │    └── supabase.js   # Supabase client setup
└── App.jsx
```

---

## 🔒 **Authentication Flow (Clerk)**

1. User lands → If not logged in → Redirect to **/?sign-in=true**
2. After login → If user has no role metadata
   → Redirect to **/onboarding**
3. Onboarding sets:

```
unsafeMetadata.role = "candidate" | "recruiter"
```

4. Protected routes check:

* must be logged in
* must have role
* recruiters can post/manage jobs
* candidates can apply/view saved jobs

---
```

## 🗂️ **Supabase Tables Overview**

### 🏢 companies

| Column   | Type |
| -------- | ---- |
| id       | int  |
| name     | text |
| logo_url | text |

### 💼 jobs

| Column       | Type     |
| ------------ | -------- |
| id           | int      |
| title        | text     |
| description  | text     |
| requirements | markdown |
| recruiter_id | uuid     |
| location     | text     |
| company_id   | int      |
| isOpen       | boolean  |

### 📁 saved_jobs

| user_id | job_id |

### 📄 applications

| Column       | Type |
| ------------ | ---- |
| id           | int  |
| name         | text |
| candidate_id | uuid |
| job_id       | int  |
| resume       | url  |
| status       | text |
| experience   | int  |
| skills       | text |
| education    | text |

---

## 🧠 **How API Layer Works (Interview Style)**

Your API functions inside `api/` follow this pattern:

```js
export async function functionName(token, params, bodyData) {
  const supabase = await supabaseClient(token);
  // Query
  return response;
}
```

This gives 3 HUGE benefits:

### Benefit 1️⃣ – **Full RLS Protection**

Supabase rules run because token = logged in user's JWT.

### Benefit 2️⃣ – **Clean React Components**

Components do NOT contain backend logic.

### Benefit 3️⃣ – **Standardized API Architecture**

---

# 🧩 **Important API Explanations**

### ⭐ 1. Apply to Job

* Upload resume to storage
* Get public URL
* Insert into applications table

### ⭐ 2. Add Company

* Upload company logo
* Insert into table

### ⭐ 3. Get Jobs

* Includes filters (location, company, searchQuery)
* Joined with `saved_jobs` and `companies`

### ⭐ 4. Save Job Toggle

* If saved → delete
* If not → insert

### ⭐ 5. Update Hiring Status

Recruiter changes job status (open/closed).

---

# 🔄 **useFetch() — Custom API Hook**

Your universal API hook:

* manages loading
* manages error
* manages token
* returns a function to execute API

```
const { loading, data, error, fn } = useFetch(apiFunction)
```

This pattern is **enterprise-grade**.

---

## 🎨 UI Architecture (ShadCN + Tailwind)

### Drawer

Used for modals (Add Company, Apply Job)

### Card

Used for JobCard, ApplicationCard

### Select

For filters, hiring status, company selection

---

# 🚀 Run Locally

### 1️⃣ Install dependencies

```
npm install
```

### 2️⃣ Add environment variables

Create `.env`:

```
VITE_CLERK_PUBLISHABLE_KEY=
VITE_CLERK_SECRET_KEY=
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

### 3️⃣ Run server

```
npm run dev
```

---

# 🌐 Deployment Steps (Vercel)

1. Push repo to GitHub
2. Import project in Vercel
3. Add same `.env` variables
4. Deploy

Supabase works automatically since URL + KEY provided.

---

# 🎥 Workflow Diagram (Simple)

```
Candidate                     Recruiter
    |                            |
    |-- browse jobs ------------>|
    |-- apply (upload resume)    |
    |                            |-- view applications
    |                            |-- update status
    |                            |
    |-- save jobs                |
```

---

# 🎉 Conclusion

This Job Portal demonstrates:

* Real-world Supabase usage
* Authentication + Authorization
* Storage uploads
* Filterable job search
* Recruiter/candidate dashboard
* Industry-level folder structure



