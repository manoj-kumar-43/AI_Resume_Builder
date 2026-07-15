# AI Resume Builder

A full-stack MERN application powered by **Google Gemini AI** that helps users create, optimize, and score resumes using a multi-agent AI system. It features conversational resume building, STAR-method bullet point generation, ATS scoring, and professional resume reviews.

## Live Demo

**Frontend:** [https://ai-resume-builder-5le3.onrender.com](https://ai-resume-builder-5le3.onrender.com)

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [API Routes](#api-routes)
- [AI Agents](#ai-agents)
- [Database Models](#database-models)

---

## Features

### Authentication
- Email/password registration and login with bcrypt hashing
- Google OAuth 2.0 sign-in
- JWT-protected routes with Bearer token authentication
- Auto-check auth on app load

### Resume Management
- Create, read, update, and delete resumes
- Update individual resume sections (personalInfo, summary, experience, education, skills, projects, certifications)
- Upload and parse existing PDF resumes into structured data
- Auto-save with 3-second debounce on the builder page
- Resume completion percentage tracking
- Version history with save, restore, and delete snapshots

### AI-Powered Features

1. **Interview Agent** - Conversational resume building through chat. Asks smart follow-up questions, probes for metrics/achievements, and autonomously updates the resume using 4 specialized tools.

2. **Bullet Writer Agent** - Transforms raw experience into STAR-method bullet points with strong action verbs and quantified results. Includes per-bullet AI improve with side-by-side diff view (Accept/Reject).

3. **ATS Scoring Engine** - 10-metric hybrid scoring (algorithmic + AI) analyzing keyword match, bullet quality, formatting, section completeness, summary strength, skill coverage, metrics, action verbs, length, and contact info. Animated score circle with color-coded thresholds.

4. **Resume Reviewer** - Section-by-section feedback with overall rating, common mistakes, strengths, and priority improvements.

5. **Job Matcher** - Match score calculation with keyword analysis, content suggestions, and overall feedback.

6. **Skill Gap Detector** - Required vs. present skills comparison with priority-ordered missing skills and recommendations.

### Resume Templates (5 designs)
- **Classic Professional** - Traditional single-column, ATS-friendly
- **Modern Tech** - Two-column with sidebar
- **Creative Bold** - Vibrant accents
- **Minimal Clean** - Elegant whitespace
- **Executive** - Dark header with serif font

### PDF Export
- Client-side PDF generation using `@react-pdf/renderer`
- Lazy-loaded for performance
- A4 page size output

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 19 | UI library |
| Vite 6 | Build tool & dev server |
| Tailwind CSS v4 | Styling |
| React Router DOM v7 | Client-side routing |
| @react-pdf/renderer v4 | PDF generation |
| @react-oauth/google | Google Sign-In |
| react-hot-toast | Toast notifications |

### Backend
| Technology | Purpose |
|---|---|
| Express.js 5 | HTTP server |
| MongoDB + Mongoose 9 | Database & ODM |
| Google Gemini AI SDK | AI content generation |
| LangChain + LangGraph | AI agent framework (Interview Agent) |
| JSON Web Token | Authentication |
| bcryptjs | Password hashing |
| pdfjs-dist | PDF text extraction |
| Multer | File upload handling |
| Zod | Schema validation |

### AI Model
- **Google Gemini 2.5 Flash**

---

## Project Structure

```
AI_Resume_Builder/
  client/                          # React frontend
    src/
      main.jsx                     # App entry: GoogleOAuth, Router, AuthProvider
      App.jsx                      # Route definitions
      context/                     # AuthContext, ResumeContext
      services/                    # API, auth, resume, AI service layers
      constants/                   # Templates, section types, ATS metrics
      pages/                       # Landing, Login, Home, Dashboard, Builder, Versions, Templates
      components/                  # UI components (see below)
        ProtectedRoute/            # Auth guard
        Navbar/                    # Top navigation
        Sidebar/                   # Builder left panel (tabs: Sections, AI Chat, ATS, Templates)
        SectionEditor/             # Accordion of 7 section forms
        PersonalInfoForm/          # Contact details form
        SummaryForm/               # Summary with AI generate
        ExperienceForm/            # Work experience with bullet points
        EducationForm/             # Education entries
        SkillsForm/                # Tag input for 3 skill categories
        ProjectsForm/              # Project entries with bullets
        CertificationsForm/        # Certification entries
        BulletPointEditor/         # Add/edit/delete bullets + AI improve
        BulletDiffView/            # Side-by-side diff with Accept/Reject
        ChatPanel/                 # AI chat interface
        ChatMessage/               # Message bubbles with "Apply to resume"
        ChatInput/                 # Auto-growing textarea
        AtsScorePanel/             # Job description + score breakdown
        AtsScoreCircle/            # Animated SVG circular gauge
        AtsChecklistItem/          # Metric row with expandable fix
        SkillGapCard/              # Missing skills display
        ProgressBar/               # Resume completion bar
        TemplateSelector/          # Template grid in builder
        ResumePreview/             # Live A4 preview + PDF download
        PdfDocument/               # React-PDF orchestrator
        pdf-templates/             # 5 PDF template components
        templates/                 # 5 HTML preview templates
        VersionList/               # Version history grid
        VersionCard/               # Version metadata + actions

  server/                          # Express backend
    server.js                      # Entry point
    src/
      app.js                       # Express app config & middleware
      config/                      # DB, Gemini, Google OAuth, Agent tools
      models/                      # User, Resume, ResumeVersion, ChatHistory
      routes/                      # auth, resumes, ai, versions
      controllers/                 # Route handlers
      services/                    # Business logic (auth, resume, ai, agent, version)
      constants/                   # AI prompt templates
      utils/                       # JWT, PDF parser, keyword analyzer, format checker, score calculator
      middleware/                   # Auth, error handling, upload (Multer)
```

---

## Getting Started

### Prerequisites
- Node.js >= 18
- MongoDB Atlas account (or local MongoDB)
- Google Gemini API key
- Google OAuth Client ID

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/AI_Resume_Builder.git
cd AI_Resume_Builder
```

### Client Setup

```bash
cd client
npm install
```

### Server Setup

```bash
cd server
npm install
```

### Running in Development

```bash
# Start the server (port 5000, auto-reload)
cd server
npm run dev

# Start the client (port 5173)
cd client
npm run dev
```

### Production Build

```bash
cd client
npm run build    # Output in client/dist/
npm run preview  # Preview production build
```

---

## Environment Variables

### Client (`client/.env`)

| Variable | Description | Default |
|---|---|---|
| `VITE_API_URL` | Backend API base URL | `https://ai-resume-builder-5le3.onrender.com/api` |
| `VITE_GOOGLE_CLIENT_ID` | Google OAuth Client ID | - |

### Server (`server/.env`)

| Variable | Description | Default |
|---|---|---|
| `PORT` | Server port | `5000` |
| `MONGODB_URI` | MongoDB connection string | - |
| `JWT_SECRET` | JWT signing secret | - |
| `JWT_EXPIRES_IN` | Token expiry duration | `7d` |
| `CLIENT_URL` | Frontend URL for CORS | `http://localhost:5173` |
| `GEMINI_API_KEY` | Google Gemini API key | - |
| `GOOGLE_CLIENT_ID` | Google OAuth Client ID | - |

---

## API Routes

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register with name/email/password |
| POST | `/api/auth/login` | Login with email/password |
| POST | `/api/auth/google` | Google OAuth login |
| GET | `/api/auth/me` | Get current user profile |
| POST | `/api/auth/logout` | Logout |

### Resumes
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/resumes` | Create new resume |
| GET | `/api/resumes` | List user resumes |
| GET | `/api/resumes/:id` | Get single resume |
| PUT | `/api/resumes/:id` | Update resume |
| PUT | `/api/resumes/:id/sections/:section` | Update single section |
| PUT | `/api/resumes/:id/template` | Update template |
| DELETE | `/api/resumes/:id` | Delete resume |
| POST | `/api/resumes/upload` | Upload & parse PDF resume |

### AI Services
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/ai/chat` | Chat with Interview Agent |
| POST | `/api/ai/generate-bullets` | Generate STAR-method bullets |
| POST | `/api/ai/generate-summary` | AI-generate summary |
| POST | `/api/ai/ats-score` | Analyze ATS score |
| POST | `/api/ai/review` | AI resume review |
| POST | `/api/ai/match-job` | Job description matching |
| POST | `/api/ai/skill-gaps` | Detect skill gaps |
| GET | `/api/ai/chat-history/:resumeId` | Get chat history |

### Versions
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/versions/:resumeId` | Save version snapshot |
| GET | `/api/versions/:resumeId` | List versions |
| GET | `/api/versions/:resumeId/:versionId` | Get single version |
| POST | `/api/versions/:resumeId/:versionId/restore` | Restore version |
| DELETE | `/api/versions/:resumeId/:versionId` | Delete version |

> All routes except auth endpoints require a valid JWT Bearer token.

---

## AI Agents

### 1. Interview Agent (LangGraph ReAct)
A conversational agent that guides users through building their resume via chat. It uses 4 autonomous tools:

| Tool | Description |
|---|---|
| `update_resume_section` | Saves gathered info directly to the database |
| `generate_star_bullets` | Creates STAR-method bullet points from raw experience |
| `get_ats_score` | Runs ATS scoring on the current resume |
| `generate_summary` | Writes a professional summary |

Maintains conversation history per resume (last 20 messages) for context.

### 2. Bullet Writer
Transforms raw work experience into polished, quantified bullet points using the STAR method (Situation, Task, Action, Result). Marks placeholder metrics with `[X]` for user customization.

### 3. ATS Scoring Engine
Hybrid scoring system that combines algorithmic analysis with AI evaluation across 10 weighted metrics:

| Metric | Weight |
|---|---|
| Keyword Match | 20% |
| Bullet Quality | 15% |
| Formatting | 10% |
| Section Completeness | 10% |
| Summary Strength | 10% |
| Skill Coverage | 10% |
| Metrics & Numbers | 10% |
| Action Verbs | 5% |
| Length | 5% |
| Contact Info | 5% |

### 4. Resume Reviewer
Provides section-by-section feedback, overall rating (strong/good/needs-work/weak), common mistakes, strengths, and top 3 priority improvements.

### 5. Job Matcher
Calculates match score against a job description and provides keyword analysis, content suggestions with section targeting, and overall feedback.

### 6. Skill Gap Detector
Compares required skills from a job description against present skills, identifies missing skills with priority levels, and provides recommendations.

---

## Database Models

### User
| Field | Type | Description |
|---|---|---|
| googleId | String | Google OAuth ID (optional) |
| email | String | Unique, required |
| name | String | Required |
| password | String | Hashed, optional (Google users) |
| picture | String | Profile picture URL |
| lastLogin | Date | Last login timestamp |

### Resume
| Field | Type | Description |
|---|---|---|
| userId | ObjectId | Reference to User |
| title | String | Resume title |
| templateId | Enum | classic/modern/creative/minimal/executive |
| targetRole | String | Target job role |
| jobDescription | String | Associated job description |
| sections | Object | 7 nested section sub-schemas |
| atsScore | Object | overall, breakdown, missingKeywords, suggestions |

### ResumeVersion
| Field | Type | Description |
|---|---|---|
| resumeId | ObjectId | Reference to Resume |
| userId | ObjectId | Reference to User |
| versionNumber | Number | Auto-incrementing |
| label | String | Custom version label |
| snapshot | Mixed | Full resume snapshot |
| templateId | String | Template at time of save |
| atsScore | Object | ATS score at time of save |

### ChatHistory
| Field | Type | Description |
|---|---|---|
| resumeId | ObjectId | Reference to Resume |
| userId | ObjectId | Reference to User |
| agentType | Enum | interview/bulletWriter/atsScorer/reviewer |
| messages | Array | [{role, content, timestamp}] |
| metadata | Object | Agent-specific metadata |

---

## License

This project is for educational purposes as part of the NxtWave AI Projects curriculum.
