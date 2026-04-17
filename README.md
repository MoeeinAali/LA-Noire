# 🕵️ LA Noire - Police Department Management System

> A modern, React-based web application for managing criminal investigations, evidence tracking, case management, and law enforcement operations. Built with TypeScript, Vite, and modern web technologies for a responsive and performant user experience.


## 🎯 Overview

LA Noire is a comprehensive criminal investigation management platform that enables law enforcement agencies to:

- **Manage Cases** - Create, track, and investigate criminal cases with detailed timelines
- **Record Evidence** - Document and categorize various types of evidence (biological, identification, vehicle, etc.)
- **Track Suspects** - Maintain suspect profiles with status tracking (wanted, arrested, convicted)
- **File Complaints** - Citizens can file formal complaints with tracking and status updates
- **Report Crime Scenes** - Officers can report and manage crime scene investigations
- **Witness Testimonies** - Collect and organize witness statements and evidence
- **Detective Board** - Visual graph-based investigation board for connecting evidence and suspects
- **Role-Based Access** - Granular permission system for different law enforcement roles
- **Reward System** - Track and manage reward reports for valuable tips
- **Admin Panel** - Comprehensive system administration and data management

## 🛠 Technology Stack

### Core Framework
- **React 19.2.0** - Modern UI library with latest features
- **TypeScript 5.9** - Strong typing for reliable code
- **Vite 7.2.4** - Lightning-fast build tool and dev server

### Routing & State Management
- **React Router v7** - Client-side routing with nested routes and guards
- **TanStack React Query v5** - Server state management with caching and synchronization
- **Zustand v5** - Lightweight client state management (authentication, UI state)

### UI & Styling
- **Tailwind CSS v4** - Utility-first CSS framework
- **Radix UI** - Unstyled, accessible component primitives
- **shadcn/ui** - High-quality React components built on Radix
- **Hugeicons** - Comprehensive icon library
- **class-variance-authority** - Type-safe component variant management

### Forms & Validation
- **React Hook Form v7** - Performant, flexible form handling
- **Zod v4** - TypeScript-first schema validation

### Data Visualization
- **@xyflow/react v12** - Interactive node-based graph visualization (Detective Board)

### Utilities
- **axios v1.13** - HTTP client for API requests
- **date-fns v4.1** - Modern date handling
- **react-toastify v11** - Toast notifications
- **html-to-image v1.11** - Export graphs/boards as images

### Testing
- **Jest v30** - JavaScript testing framework
- **@testing-library/react** - React component testing utilities
- **@testing-library/user-event** - User interaction simulation

### Development Tools
- **pnpm** - Fast, space-efficient package manager
- **ESLint** - Code quality and style enforcement
- **TypeScript ESLint** - TypeScript-specific linting

## 🏗 Architecture

### High-Level Architecture

```mermaid
graph TB
    Client["🌐 Browser Client"]
    Router["🛣️ React Router"]
    Auth["🔐 Auth Store<br/>Zustand"]
    Pages["📄 Page Components"]
    Components["🧩 UI Components"]
    Query["📊 React Query<br/>Server Cache"]
    API["🔌 API Layer<br/>Axios"]
    Backend["🔌 Backend API<br/>Django REST"]
    
    Client --> Router
    Router --> Pages
    Pages --> Components
    Pages --> Auth
    Pages --> Query
    Query --> API
    Auth --> API
    API --> Backend
    
    style Client fill:#1e40af
    style Router fill:#7c3aed
    style Auth fill:#c084fc
    style Pages fill:#6366f1
    style Components fill:#a78bfa
    style Query fill:#f97316
    style API fill:#ef4444
    style Backend fill:#16a34a
```

### Component Hierarchy

```mermaid
graph TD
    App["App<br/>BrowserRouter"]
    MainLayout["MainLayout<br/>Header + Outlet"]
    RoleGuard["RoleGuard<br/>Protected Routes"]
    AdminLayout["AdminLayout<br/>Admin Routes"]
    Pages["Feature Pages<br/>cases, evidence, etc."]
    UI["UI Components<br/>Form, Dialog, Table, etc."]
    
    App --> MainLayout
    App --> AdminLayout
    MainLayout --> RoleGuard
    RoleGuard --> Pages
    Pages --> UI
    AdminLayout --> Pages
    
    style App fill:#1e40af
    style MainLayout fill:#7c3aed
    style AdminLayout fill:#7c3aed
    style RoleGuard fill:#c084fc
    style Pages fill:#f97316
    style UI fill:#a78bfa
```

### Data Flow (Server State Management)

```mermaid
sequenceDiagram
    participant React as React Component
    participant Query as React Query Cache
    participant API as API Layer
    participant Backend as Django Backend
    
    React->>Query: useQuery / useMutation
    Query->>API: HTTP Request (if not cached)
    API->>Backend: Axios Request
    Backend-->>API: JSON Response
    API-->>Query: Transform & Cache
    Query-->>React: Trigger Re-render
    React->>Query: useQuery (next render)
    Query-->>React: Cached Data (instant)
```

### State Management Architecture

```mermaid
graph LR
    Auth["🔐 Auth Store<br/>Session Data<br/>Access Token"]
    DetectiveBoard["🕵️ Detective Board<br/>Node/Edge State"]
    UI["🎨 UI State<br/>Filters, Loading"]
    Server["📡 Server State<br/>Cases, Evidence<br/>Suspects, etc."]
    
    Auth -->|Provide Token| Server
    Server <-->|React Query| Cache["Cache Layer"]
    UI -.->|Client Only| React
    DetectiveBoard -.->|Zustand| React
    
    style Auth fill:#c084fc
    style DetectiveBoard fill:#fbbf24
    style UI fill:#a78bfa
    style Server fill:#f97316
    style Cache fill:#fcd34d
    style React fill:#6366f1
```

### Role-Based Access Control Flow

```mermaid
graph TD
    User["Authenticated User<br/>Session Data"]
    RoleGuard["RoleGuard Component<br/>Check User Role"]
    AllowedRoles["Allowed Roles for Route<br/>Detective, Captain, etc."]
    
    Decision{Role in<br/>AllowedRoles?}
    
    Protected["✅ Access Granted<br/>Render Protected Route"]
    Denied["❌ Access Denied<br/>Redirect to Home"]
    
    User --> RoleGuard
    RoleGuard --> AllowedRoles
    AllowedRoles --> Decision
    Decision -->|Yes| Protected
    Decision -->|No| Denied
    
    style User fill:#1e40af
    style RoleGuard fill:#c084fc
    style AllowedRoles fill:#fbbf24
    style Protected fill:#16a34a
    style Denied fill:#dc2626
```

## 📁 Project Structure

```
la-noire/
├── src/
│   ├── pages/                    # Page components organized by feature
│   │   ├── cases/               # Case management pages
│   │   │   ├── cases-list.page.tsx
│   │   │   ├── create-case.page.tsx
│   │   │   ├── case-detail.page.tsx
│   │   │   ├── case-timeline.page.tsx
│   │   │   ├── add-suspect.page.tsx
│   │   │   ├── manage-suspects.page.tsx
│   │   │   └── review-suspects.page.tsx
│   │   ├── evidence/            # Evidence recording and tracking
│   │   │   ├── case-evidence.page.tsx
│   │   │   ├── record-evidence.page.tsx
│   │   │   └── evidence-detail.page.tsx
│   │   ├── complaints/          # Complaint filing system
│   │   │   ├── complaints-list.page.tsx
│   │   │   ├── file-complaint.page.tsx
│   │   │   └── complaint-detail.page.tsx
│   │   ├── crime-scenes/        # Crime scene reporting
│   │   │   ├── crime-scenes-list.page.tsx
│   │   │   ├── report-crime-scene.page.tsx
│   │   │   └── crime-scene-detail.page.tsx
│   │   ├── testimonies/         # Witness testimonies
│   │   │   ├── testimonies-list.page.tsx
│   │   │   └── testimony-detail.page.tsx
│   │   ├── detective-board/     # Visual investigation board
│   │   │   └── detective-board.page.tsx
│   │   ├── reward/              # Reward system pages
│   │   │   ├── reward-reports.page.tsx
│   │   │   ├── my-reports.page.tsx
│   │   │   ├── my-rewards.page.tsx
│   │   │   └── claim-reward.page.tsx
│   │   ├── admin/               # Admin panel pages
│   │   │   ├── admin-dashboard.page.tsx
│   │   │   ├── admin-cases.page.tsx
│   │   │   ├── admin-roles.page.tsx
│   │   │   ├── admin-users.page.tsx
│   │   │   ├── admin-suspects.page.tsx
│   │   │   ├── admin-complaints.page.tsx
│   │   │   ├── admin-crime-scenes.page.tsx
│   │   │   └── admin-punishments.page.tsx
│   │   ├── court/               # Court system (Judges)
│   │   │   ├── court-dashboard.page.tsx
│   │   │   └── trial.page.tsx
│   │   ├── login.page.tsx
│   │   ├── register.page.tsx
│   │   ├── home.page.tsx
│   │   ├── roles.page.tsx       # Role management
│   │   └── payment.page.tsx     # Payment processing
│   │
│   ├── components/              # Reusable UI components
│   │   ├── ui/                 # Base UI components (Radix + Tailwind)
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── select.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── table.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── card.tsx
│   │   │   ├── tabs.tsx
│   │   │   ├── accordion.tsx
│   │   │   ├── skeleton.tsx
│   │   │   └── ...
│   │   ├── layout/              # Layout components
│   │   │   ├── main-layout.tsx
│   │   │   ├── admin-layout.tsx
│   │   │   ├── header.tsx
│   │   │   └── root-error-boundary.tsx
│   │   ├── cases/               # Case-specific components
│   │   │   ├── case-header.tsx
│   │   │   ├── case-filters.tsx
│   │   │   ├── case-empty-state.tsx
│   │   │   └── case-card.tsx
│   │   ├── evidence/            # Evidence form components
│   │   │   ├── biological-evidence-form.tsx
│   │   │   ├── vehicle-evidence-form.tsx
│   │   │   ├── identification-evidence-form.tsx
│   │   │   ├── other-evidence-form.tsx
│   │   │   └── testimony-form.tsx
│   │   ├── detective-board/     # Detective board visualization
│   │   │   └── nodes/
│   │   │       ├── note-node.tsx
│   │   │       └── evidence-node.tsx
│   │   ├── complaints/          # Complaint components
│   │   │   ├── complaint-item.tsx
│   │   │   └── complaint-status-badge.tsx
│   │   ├── testimonies/         # Testimony components
│   │   │   └── testimony-card.tsx
│   │   ├── admin/               # Admin components
│   │   │   └── admin-table.tsx
│   │   ├── rewards/             # Reward components
│   │   │   └── reward-card.tsx
│   │   └── wanted-suspects/     # Wanted list components
│   │       └── report-tip-dialog.tsx
│   │
│   ├── api/                     # API service layer
│   │   ├── auth.ts             # Authentication endpoints
│   │   ├── cases.ts            # Case management endpoints
│   │   ├── evidence.ts         # Evidence endpoints
│   │   ├── suspects.ts         # Suspect endpoints
│   │   ├── complaints.ts       # Complaint endpoints
│   │   ├── crime-scenes.ts     # Crime scene endpoints
│   │   ├── interrogations.ts   # Interrogation endpoints
│   │   ├── testimonies.ts      # Testimony endpoints
│   │   ├── reward-reports.ts   # Reward report endpoints
│   │   ├── rewards.ts          # Reward endpoints
│   │   ├── roles.ts            # Role endpoints
│   │   ├── payment.ts          # Payment endpoints
│   │   └── punishment.ts       # Punishment endpoints
│   │
│   ├── stores/                  # Zustand state management
│   │   ├── auth.store.ts       # Authentication state
│   │   └── detective-board.store.ts # Detective board state
│   │
│   ├── types/                   # TypeScript type definitions
│   │   ├── auth.type.ts
│   │   ├── role.type.ts
│   │   ├── evidence.type.ts
│   │   ├── complaint.type.ts
│   │   └── reward-report.type.ts
│   │
│   ├── schemas/                 # Zod validation schemas
│   │   ├── auth.schema.ts
│   │   ├── evidence.schema.ts
│   │   ├── role.schema.ts
│   │   └── payment.schema.ts
│   │
│   ├── guards/                  # Route guards
│   │   └── role.guard.tsx      # Role-based access control
│   │
│   ├── hooks/                   # Custom React hooks
│   │   └── useApiError.ts      # Error handling hook
│   │
│   ├── lib/                     # Utility functions
│   │   ├── http.ts             # HTTP client configuration
│   │   └── utils.ts            # Helper functions
│   │
│   ├── providers/               # Context/Provider components
│   │   └── query-provider.tsx  # React Query provider setup
│   │
│   ├── __tests__/               # Test files
│   │   ├── components/
│   │   ├── stores/
│   │   ├── schemas/
│   │   └── lib/
│   │
│   ├── index.css               # Global styles
│   ├── main.tsx                # Application entry point
│   └── App.tsx                 # Main app component with routing
│
├── _docs/
│   └── figs/                   # Screenshots and documentation images
│
├── public/                      # Static assets
├── .dockerignore
├── Dockerfile                  # Docker containerization
├── docker-compose.yml          # Docker Compose setup
├── jest.config.ts              # Jest testing configuration
├── vite.config.ts              # Vite build configuration
├── tsconfig.json               # TypeScript configuration
├── tsconfig.app.json
├── tsconfig.node.json
├── eslint.config.js            # ESLint configuration
├── package.json                # Dependencies and scripts
├── pnpm-lock.yaml              # Dependency lock file
└── README.md                   # This file
```

## ✨ Features

### 1. **Case Management**
   - Create new cases with detailed information
   - Track case status and investigation progress
   - View case timeline with all activities
   - Add and manage suspects linked to cases
   - Review suspect progression through case workflow

### 2. **Evidence Management**
   - Record multiple types of evidence:
     - **Biological Evidence** - DNA samples, blood, tissue
     - **Vehicle Evidence** - License plates, VINs, registration
     - **Identification Evidence** - IDs, documents
     - **Other Evidence** - Generic categorization
   - Track evidence chain of custody
   - Link evidence to specific cases
   - View detailed evidence reports

### 3. **Detective Board**
   - Interactive graph visualization with `@xyflow/react`
   - Create note nodes for investigation notes
   - Connect evidence and suspects visually
   - Export board as images for reports
   - Drag-and-drop node organization

### 4. **Suspect Management**
   - Maintain suspect database with profiles
   - Track suspect status (wanted, arrested, convicted)
   - Link suspects to cases
   - View suspect interrogations and testimonies
   - Track suspect progression through legal system

### 5. **Complaint System**
   - File official complaints
   - Track complaint status
   - Maintain complaint details and evidence
   - View complaint history and updates

### 6. **Crime Scene Management**
   - Report new crime scenes
   - Document crime scene details
   - Track investigation status
   - Manage multiple scenes for single case

### 7. **Witness Testimonies**
   - Collect witness statements
   - Link testimonies to cases
   - Track testimony dates and updates
   - Maintain complete testimony records

### 8. **Reward System**
   - Report tips for wanted suspects
   - Track reward report status
   - Claim approved rewards
   - Manage reward transactions

### 9. **Role-Based Access Control**
   - Administrator - Full system access, user management
   - Chief - Supervise cases and personnel
   - Captain - Manage teams and investigations
   - Detective - Investigate cases, record evidence
   - Sergeant - Oversee operations
   - Police/Patrol Officer - Report incidents, file complaints
   - Judge - Review cases and conduct trials
   - Public - File complaints, report tips

### 10. **Admin Panel**
   - Dashboard with system statistics
   - User management and role assignment
   - Case and suspect administration
   - Complaint and crime scene management
   - Punishment tracking
   - System configuration

### 11. **Court System**
   - Judge access to trial dashboard
   - Case trial management
   - Verdict recording and case closure

## 🚀 Installation & Setup

### Prerequisites
- Node.js 18+ 
- pnpm 10+
- Backend API running at `http://localhost:8000`

### Quick Start

1. **Clone and navigate to frontend directory**
   ```bash
   cd /Users/moeein/Desktop/la-noire
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Configure environment variables**
   Create a `.env.local` file in the project root:
   ```env
   VITE_API_BASE_URL=http://localhost:8000/api
   VITE_API_TIMEOUT=30000
   ```

4. **Start development server**
   ```bash
   pnpm run dev
   ```
   The application will be available at `http://localhost:5173`

5. **Access the application**
   - **Homepage**: `http://localhost:5173`
   - **Login**: Use backend credentials created during backend setup
   - **Admin Panel**: Available at `/admin` for Administrator role

## 🔧 Development

### Development Server
```bash
pnpm run dev
```
Starts Vite dev server with hot module replacement (HMR) for instant updates.

### Building for Production
```bash
pnpm run build
```
Creates an optimized production build in the `dist/` directory.

### Code Quality
```bash
pnpm run lint
```
Runs ESLint to check code style and quality. Fix automatically with:
```bash
pnpm run lint -- --fix
```

### Preview Production Build
```bash
pnpm run preview
```
Serves the production build locally for testing.

## 🧪 Testing

### Run Tests
```bash
pnpm run test
```
Runs Jest test suite once.

### Watch Mode
```bash
pnpm run test:watch
```
Re-runs tests automatically when files change.

### Coverage Report
```bash
pnpm run test:coverage
```
Generates coverage report for analyzed files.

## 🐳 Docker Deployment

### Build Docker Image
```bash
docker build -t la-noire-frontend .
```

### Run Container
```bash
docker run -p 3000:80 la-noire-frontend
```

### Using Docker Compose
```bash
docker-compose up
```

## 🔌 API Integration

The frontend communicates with the Django REST Backend via HTTP (Axios). Key API services:

- **Authentication** - Login, register, token management
- **Cases** - CRUD operations, case status, timeline
- **Evidence** - Record, categorize, track evidence items
- **Suspects** - Profile management, status tracking
- **Complaints** - Filing and tracking
- **Crime Scenes** - Reporting and management
- **Testimonies** - Collection and storage
- **Rewards** - Tip reporting and claim processing
- **Users** - Profile management (admin)
- **Roles** - Role configuration (admin)

API Base URL: `http://localhost:8000/api`

## 🎨 Screenshots

### Landing Page
Initial landing page showing navigation and feature overview.

![Landing Page](_docs/figs/landing-page.png)

### Cases Management
View and manage all cases with filters and search.

![Cases Page](_docs/figs/cases-page.png)

### Case Timeline
Visual timeline of all case activities and updates.

![Case Timeline](_docs/figs/case-timeline.png)

### Detective Board
Interactive graph-based investigation visualization using React Flow.

![Detective Board](_docs/figs/detective-board.png)

![Detective Board React Flow](_docs/figs/detective-board-react-flow.png)

### Complaint System
File and track official complaints.

![Complaints Page](_docs/figs/complaints-page.png)

![Complaints Detail](_docs/figs/complaints-detail-page.png)

![Submit Complaint](_docs/figs/complaints-skeleton.png)

### Crime Scenes
Report and manage crime scene investigations.

![Crime Scenes](_docs/figs/crime-scenes-page.png)

![Report Crime Scene](_docs/figs/report-crime-scene-page.png)

![Crime Scene Confirm/Delete](_docs/figs/crime-scene-confirm-or-delete.png)

### Evidence Recording
Record different types of evidence for investigations.

![Record Evidence](_docs/figs/record-evidence-for-case.png)

### Witness Testimonies
Collect and manage witness statements.

![Testimonies List](_docs/figs/witness-testimonies-page.png)

![Witness Statement](_docs/figs/witness-statement.png)

![Submit Testimony](_docs/figs/submit-testimony.png)

### Reports and Rewards
Manage reward reports and claims.

![Reward Reports](_docs/figs/reward-reports-page.png)

![Report Detail](_docs/figs/report-detail.png)

### User Profiles
User profile management and information.

![Profile Page](_docs/figs/profile-page.png)

![Profile Skeleton](_docs/figs/profile-page-skeleton.png)

### Payment System
Process payments for rewards.

![Purchase Coupon](_docs/figs/purchase-coupon.png)

### Admin Panel
Comprehensive admin dashboard and management tools.

![Admin Dashboard](_docs/figs/admin-panel-0.png)

![Admin Management 1](_docs/figs/admin-panel-1.png)

![Admin Management 2](_docs/figs/admin-panel-2.png)

![Admin Roles](_docs/figs/role-management.png)

### Backend Integration
Backend API administrative interface.

![Backend Admin](_docs/figs/localhost_8000_admin_.png)

## 📦 Key Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| react | 19.2.0 | UI library |
| react-router-dom | 7.13.0 | Client-side routing |
| @tanstack/react-query | 5.90.21 | Server state management |
| zustand | 5.0.11 | Client state management |
| react-hook-form | 7.71.2 | Form handling |
| zod | 4.3.6 | Schema validation |
| tailwindcss | 4.1.17 | Utility CSS |
| @xyflow/react | 12.10.1 | Graph visualization |
| @radix-ui/react-tabs | 1.1.13 | Accessible components |
| shadcn | 3.8.5 | Component library |
| axios | 1.13.5 | HTTP client |
| react-toastify | 11.0.5 | Notifications |
| jest | 30.2.0 | Testing framework |

## 🔐 Security & Best Practices

- **Type Safety**: Full TypeScript coverage for compile-time type checking
- **Validation**: Zod schemas for runtime data validation
- **Protected Routes**: Role-based access control with RoleGuard component
- **Token Management**: Secure authentication token handling via Zustand
- **Error Handling**: Global error boundary and API error handling
- **Input Sanitization**: React prevents XSS through default escaping
- **HTTPS Ready**: Production builds support secure connections
