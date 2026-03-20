# 🚀 TaskFlow — Task Management System

> Built for **Earnest Data Analytics** Software Engineering Assessment  
> Track A: Full-Stack Engineer (Backend + Web Frontend)

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Visit%20App-brightgreen?style=for-the-badge)](https://task-manager-coral-mu.vercel.app/)
[![Backend API](https://img.shields.io/badge/Backend%20API-Live-blue?style=for-the-badge)](https://task-manager-backend-emge.onrender.com)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-black?style=for-the-badge&logo=github)](https://github.com/SonuMandal1/Task-Management-System-)

---

## 📌 About This Project

TaskFlow is a **production-ready, full-stack Task Management System** built as part of the **Earnest Data Analytics Software Engineering Recruitment Assessment**. The system allows users to securely register, log in, and perform complete management of their personal tasks with a beautiful, responsive interface.

The project is **fully deployed and live** — both the backend API and the frontend web application are running in production.

---

## 🌐 Live Deployment

| Service | URL |
|---------|-----|
| 🌐 **Frontend (Next.js)** |  https://task-manager-coral-mu.vercel.app/ |
| ⚙️ **Backend API (Node.js)** | https://task-manager-backend-emge.onrender.com |
| 🗄️ **Database** | Neon PostgreSQL (Cloud) |

---

## ✨ Features Implemented

### 🔐 Authentication & Security
- ✅ User Registration with full validation
- ✅ User Login with email & password
- ✅ JWT Access Tokens (short-lived, 15 minutes)
- ✅ JWT Refresh Tokens (long-lived, 7 days) stored in HttpOnly cookies
- ✅ Automatic token refresh — user stays logged in seamlessly
- ✅ Token rotation on every refresh for maximum security
- ✅ Password hashing using **bcrypt** (12 salt rounds)
- ✅ Secure Logout with token revocation

### 📋 Task Management (Full CRUD)
- ✅ Create tasks with title, description, priority and due date
- ✅ View all tasks with beautiful card layout
- ✅ Edit tasks via modal form
- ✅ Delete tasks with confirmation dialog
- ✅ Toggle task status (Pending → In Progress → Completed)
- ✅ Task priority levels — Low, Medium, High
- ✅ Due date tracking with overdue detection

### 🔍 Advanced Task Features
- ✅ **Pagination** — tasks loaded in batches
- ✅ **Search** — search tasks by title (debounced)
- ✅ **Filter** — filter by status and priority
- ✅ **Real-time stats** — total, pending, in-progress, completed counts

### 🎨 UI/UX
- ✅ Fully **responsive design** — works on mobile and desktop
- ✅ Beautiful dark theme with glass morphism
- ✅ Toast notifications for all actions
- ✅ Loading states and smooth animations
- ✅ Confirmation dialogs for destructive actions
- ✅ Empty states with helpful prompts

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|-----------|---------|
| **Node.js** | Server runtime |
| **Express.js** | Web framework |
| **TypeScript** | Type safety throughout |
| **Prisma ORM** | Database access & migrations |
| **PostgreSQL (Neon)** | Production database |
| **JWT (jsonwebtoken)** | Access & refresh token auth |
| **bcryptjs** | Password hashing |
| **express-validator** | Input validation |
| **cookie-parser** | HttpOnly cookie handling |

### Frontend
| Technology | Purpose |
|-----------|---------|
| **Next.js 14** | React framework (App Router) |
| **TypeScript** | Type safety throughout |
| **Tailwind CSS** | Utility-first styling |
| **React Hook Form** | Form handling |
| **Zod** | Schema validation |
| **Axios** | HTTP client with interceptors |
| **react-hot-toast** | Toast notifications |
| **lucide-react** | Icon library |

### DevOps & Deployment
| Technology | Purpose |
|-----------|---------|
| **Vercel** | Frontend deployment |
| **Render** | Backend deployment |
| **Neon** | Cloud PostgreSQL database |
| **GitHub** | Version control |

---

## 📁 Project Structure
```
task-manager/
├── backend/                  # Node.js + TypeScript API
│   ├── prisma/
│   │   └── schema.prisma     # Database schema
│   ├── src/
│   │   ├── controllers/
│   │   │   ├── auth.controller.ts
│   │   │   └── task.controller.ts
│   │   ├── middleware/
│   │   │   ├── auth.middleware.ts
│   │   │   ├── error.middleware.ts
│   │   │   └── validation.middleware.ts
│   │   ├── routes/
│   │   │   ├── auth.routes.ts
│   │   │   └── task.routes.ts
│   │   ├── lib/
│   │   │   ├── prisma.ts
│   │   │   └── jwt.ts
│   │   └── index.ts
│   └── package.json
│
└── frontend/                 # Next.js 14 + TypeScript
    ├── src/
    │   ├── app/
    │   │   ├── (app)/
    │   │   │   ├── login/
    │   │   │   ├── register/
    │   │   │   └── dashboard/
    │   ├── components/
    │   │   ├── TaskCard.tsx
    │   │   ├── TaskModal.tsx
    │   │   ├── DeleteDialog.tsx
    │   │   └── StatsCard.tsx
    │   ├── contexts/
    │   │   └── AuthContext.tsx
    │   └── lib/
    │       ├── api.ts
    │       ├── auth.ts
    │       └── tasks.ts
    └── package.json
```

---

## 🔌 API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register new user |
| POST | `/auth/login` | Login user |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Logout user |

### Tasks (All require authentication)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/tasks` | Get all tasks (paginated, filterable) |
| POST | `/tasks` | Create new task |
| GET | `/tasks/:id` | Get single task |
| PATCH | `/tasks/:id` | Update task |
| DELETE | `/tasks/:id` | Delete task |
| PATCH | `/tasks/:id/toggle` | Toggle task status |

### Query Parameters for GET /tasks
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | number | Page number |
| `limit` | number | Items per page |
| `status` | string | Filter by PENDING, IN_PROGRESS, COMPLETED |
| `priority` | string | Filter by LOW, MEDIUM, HIGH |
| `search` | string | Search by task title |

---

## 🗄️ Database Schema
```prisma
model User {
  id            String   @id @default(cuid())
  email         String   @unique
  name          String
  passwordHash  String
  tasks         Task[]
  refreshTokens RefreshToken[]
}

model Task {
  id          String    @id @default(cuid())
  title       String
  description String?
  status      String    @default("PENDING")
  priority    String    @default("MEDIUM")
  dueDate     DateTime?
  userId      String
  user        User      @relation(...)
}

model RefreshToken {
  id        String   @id @default(cuid())
  token     String   @unique
  userId    String
  expiresAt DateTime
}
```

---

## 🚀 Running Locally (Step by Step)

### Prerequisites
- **Node.js** v18+ — https://nodejs.org
- **Git** — https://git-scm.com
- **VS Code** — https://code.visualstudio.com

### Step 1 — Clone the repository
```bash
git clone https://github.com/SonuMandal1/Task-Management-System-.git
cd Task-Management-System-
```

### Step 2 — Setup Backend
```bash
cd backend
npm install
```

Create `.env` file inside the `backend` folder:
```env
DATABASE_URL="file:./dev.db"
JWT_ACCESS_SECRET="your-access-secret-key"
JWT_REFRESH_SECRET="your-refresh-secret-key"
JWT_ACCESS_EXPIRES_IN="15m"
JWT_REFRESH_EXPIRES_IN="7d"
PORT=5000
NODE_ENV="development"
CORS_ORIGIN="http://localhost:3000"
```

Run database migrations:
```bash
npx prisma generate
npx prisma migrate dev --name init
```

Start the backend:
```bash
npm run dev
```
✅ Backend running at **http://localhost:5000**

### Step 3 — Setup Frontend
Open a **new terminal** and run:
```bash
cd frontend
npm install
```

Create `.env.local` file inside the `frontend` folder:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

Start the frontend:
```bash
npm run dev
```
✅ Frontend running at **http://localhost:3000**

### Step 4 — Open in browser
```
http://localhost:3000
```

---

## 🔒 Security Features

- **JWT Access Tokens** — expire in 15 minutes
- **JWT Refresh Tokens** — expire in 7 days, stored in HttpOnly cookies
- **Token Rotation** — new refresh token issued on every refresh
- **bcrypt** — passwords hashed with 12 salt rounds
- **Input Validation** — all inputs validated on both frontend and backend
- **CORS Protection** — only allowed origins can access the API
- **User Isolation** — users can only access their own tasks

---

## 👨‍💻 Developer

**Sonu Mandal**  
GitHub: [@SonuMandal1](https://github.com/SonuMandal1)  
Email: 22054371@kiit.ac.in

---

## 📄 Assignment Details

- **Company:** Earnest Data Analytics  
- **Role:** Full-Stack Engineer  
- **Track:** Track A — Backend (Node.js) + Web Frontend (Next.js)  
- **Assessment:** Task Management System

---

*Built with ❤️ for Earnest Data Analytics Recruitment Drive*
```

---

