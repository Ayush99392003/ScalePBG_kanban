# KanbanFlow — Team Kanban & Sprint Management Platform

An enterprise-grade, full-stack project management platform with real-time collaboration, sprint planning, role-based access control, and analytics.

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18 + TypeScript + Vite |
| **State** | Zustand + TanStack React Query |
| **Drag & Drop** | @dnd-kit/core + @dnd-kit/sortable |
| **Charts** | Recharts |
| **Icons** | Lucide React |
| **Backend** | Express.js + TypeScript |
| **Auth** | Firebase Authentication (Email/Password + Google SSO) |
| **Database** | Firebase Firestore (real-time onSnapshot) |
| **Monorepo** | npm workspaces |

---

## Project Structure

```
kanban/
├── package.json              # Root npm workspaces config
├── .env.example              # Environment variable template
├── firestore.rules           # Firestore Security Rules
├── firestore.indexes.json    # Composite indexes
├── packages/
│   ├── client/               # Vite + React SPA
│   │   └── src/
│   │       ├── firebase/     # Firebase client config
│   │       ├── services/     # Axios API client
│   │       ├── store/        # Zustand stores
│   │       ├── hooks/        # Firestore realtime hooks
│   │       ├── components/   # UI components
│   │       │   ├── kanban/   # Board, Column, TaskCard, CardDetailModal
│   │       │   ├── sprint/   # BurndownChart
│   │       │   ├── views/    # AdminDashboard
│   │       │   └── common/   # Avatar, Badge, Modal, ...
│   │       └── pages/        # LoginPage, OnboardingPage, BoardPage
│   └── server/               # Express API
│       └── src/
│           ├── firebase/     # Firebase Admin SDK
│           ├── types/        # Domain entity types
│           ├── interfaces/   # Repository contracts
│           ├── repositories/ # Firestore implementations
│           ├── auth/         # Middleware + RBAC engine
│           └── routers/      # Auth, Org, Project, Sprint, Task
└── docs/                     # Architecture documentation
```

---

## Setup

### Prerequisites
- Node.js 18+
- A Firebase project with **Firestore**, **Firebase Authentication** enabled

### 1. Clone & Install

```bash
git clone <repo>
cd kanban
npm install
```

### 2. Configure Environment Variables

Copy `.env.example` and create:
- `packages/server/.env` — Firebase Admin SDK credentials
- `packages/client/.env` — Firebase Web SDK config + API URL

```bash
# Server
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=your-sa@project.iam.gserviceaccount.com
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
PORT=3001
CLIENT_ORIGIN=http://localhost:5173

# Client
VITE_FIREBASE_API_KEY=...
VITE_FIREBASE_AUTH_DOMAIN=...
VITE_FIREBASE_PROJECT_ID=...
VITE_FIREBASE_STORAGE_BUCKET=...
VITE_FIREBASE_MESSAGING_SENDER_ID=...
VITE_FIREBASE_APP_ID=...
VITE_API_BASE_URL=http://localhost:3001/api/v1
```

### 3. Deploy Firestore Rules & Indexes (optional)

```bash
firebase deploy --only firestore:rules,firestore:indexes
```

### 4. Run

```bash
npm run dev
```

- Client: http://localhost:5173
- Server: http://localhost:3001/api/v1

---

## Onboarding Flow

1. **Register** via Email/Password or Google SSO
2. **Create Organization** — first user becomes Admin
   - Or **Request Access** to an existing org by slug
3. Admin reviews and **approves/rejects** access requests in the Dashboard
4. Once approved, members gain access and can view/edit tasks

---

## Features

### Kanban Board
- 5-column board: Backlog → To Do → In Progress → In Review → Done
- Drag-and-drop task cards between columns (dnd-kit)
- WIP limit indicators with warning badges
- Optimistic UI updates with version conflict detection and rollback
- Real-time sync via Firestore `onSnapshot`

### Task Management
- Rich task detail modal with tabs: Overview, Comments, Subtasks
- Priority badges (Low / Medium / High / Urgent)
- Subtask checklist with progress bar
- Comment threads with user avatars

### Sprint Planning
- Create sprints with start/end dates and goals
- Move tasks from backlog into sprint
- Activate / close sprints with status transitions
- Burndown chart (ideal vs actual remaining work)

### Admin Dashboard
- Pending access request queue with Approve/Reject
- Team capacity bar chart
- Member management with role promotion

### Role-Based Access Control (RBAC)

| Action | Admin | Lead | Member |
|---|---|---|---|
| Create/delete project | ✅ | ❌ | ❌ |
| Create/close sprint | ✅ | ✅ | ❌ |
| Add tasks to sprint | ✅ | ✅ | ❌ |
| Create tasks | ✅ | ✅ | ✅ |
| Edit assigned tasks | ✅ | ✅ | Own only |
| View admin dashboard | ✅ | ✅ | ❌ |
| Review access requests | ✅ | ❌ | ❌ |

---

## Git Commit Guidelines

Follow Conventional Commits:
- `feat:` — new features
- `fix:` — bug fixes
- `docs:` — documentation
- `refactor:` — code restructuring
- `chore:` — dependency/config updates
