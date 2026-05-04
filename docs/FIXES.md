# Implementation Plan

This document describes the minimum frontend and backend pieces needed to make the Tenant Rent Payment System functional, plus a few practical recommendations for project hygiene.

## 1. Current repo status

The repository currently has:
- A backend `package.json` with dependencies and scripts
- `backend/server.js` with a basic Express app skeleton
- GitHub Actions workflow for Azure deployment
- `docs/DEPLOYMENT.md` describing deployment concepts

What is missing:
- Frontend app folder and source files
- Most backend folders and app modules described in `README.md`
- `backend/.env.example`
- migration/seed scripts (`migrations/run.js`, `seeders/seed.js`)
- any actual route/controller/model implementation

## 2. Frontend implementation basics

### 2.1 Recommended structure

Create a `frontend/` folder with at least these subfolders:
- `frontend/src/components/` — reusable UI pieces
- `frontend/src/pages/` — page views like login, dashboard, payment form
- `frontend/src/services/` — API client and request helpers
- `frontend/src/hooks/` — custom hooks for auth and data fetching
- `frontend/src/store/` or `frontend/src/redux/` — app state management
- `frontend/src/assets/` — images and styles

### 2.2 Minimum screens

At minimum implement these pages:
- `Login` / `Register`
- `Dashboard` for tenants and landlords
- `PayRent` form
- `PaymentHistory` listing past payments
- `ReceiptView` or download page
- `Profile` / account settings

### 2.3 Frontend behavior

The frontend should:
- call backend APIs through a shared `api.js` client
- store auth tokens securely in memory or `httpOnly` cookies
- protect routes with a `ProtectedRoute` wrapper
- handle loading and error states clearly
- validate form inputs before submission


## 3. Backend implementation basics

### 3.1 Recommended structure

Implement the backend inside `backend/` with these folders:
- `backend/config/` — database and app configuration
- `backend/models/` — Sequelize models
- `backend/routes/` — Express route definitions
- `backend/controllers/` — route handlers and business logic
- `backend/middleware/` — auth, validation, error handling
- `backend/utils/` — helpers like email, PDF generation, payment integration
- `backend/migrations/` — table creation scripts
- `backend/seeders/` — sample data seed scripts

### 3.2 Required backend modules

The server should include:
- `auth` flow with JWT login and registration
- user roles: tenant, landlord, admin
- `payment` flow for pay rent and payment history
- `receipt` generation with PDF output
- property/tenant management endpoints
- database ORM integration with Sequelize

### 3.3 Essential middleware

- Add essential auth and error handling middleware to routes.
- Add validation middleware to routes.


## 4. Suggested backend endpoints

Use a base route like `/api` and add these grouped endpoints:

### Authentication
- `POST /api/auth/register` — new user registration
- `POST /api/auth/login` — issue access token
- `POST /api/auth/logout` — revoke session or token
- `GET /api/auth/me` — current user profile

### Users and roles
- `GET /api/users` — list users (admin)
- `GET /api/users/:id` — single user details
- `PUT /api/users/:id` — update profile or role
- `DELETE /api/users/:id` — delete account (admin)

### Properties and tenants
- `GET /api/properties` — list properties
- `POST /api/properties` — create new property
- `PUT /api/properties/:id` — update property
- `DELETE /api/properties/:id` — remove property
- `GET /api/tenants` — list tenants
- `POST /api/tenants` — add tenant
- `PUT /api/tenants/:id` — update tenant
- `DELETE /api/tenants/:id` — remove tenant

### Payments and receipts
- `POST /api/payments` — create a rent payment
- `GET /api/payments` — list payments for a user
- `GET /api/payments/:id` — payment details
- `GET /api/receipts/:id` — download receipt PDF
- `GET /api/receipts` — list receipts for user

## 5. Practical project-level recommendations

### 5.1 Use a lockfile and package manager consistently

Prefer `pnpm` for faster installs and smaller disk usage, but keep either one consistent across the project.
- `pnpm install` instead of `npm install`
- commit `pnpm-lock.yaml`
- if you use npm, commit `package-lock.json`

### 5.2 Keep dependencies explicit and minimal

Install only what you need. Avoid packages that are not used in code.

### 5.3 Use GitHub Actions correctly

The current workflow is generic and assumes root package installation. Update it.

### 5.4 Use testing and linting

Add basic tests for auth, payments, and route behavior.
- backend: Jest + Supertest
- frontend: React Testing Library or Jest

### 5.5 Keep documentation accurate

The README should reflect the repo contents. If the frontend is not present, remove or update frontend instructions until the frontend exists.

## 6. Minimal next steps

1. Add `frontend/` skeleton and API client.
2. Add `backend/.env.example`.
3. Add backend folders and at least one real route file.
4. Wire up `backend/routes/auth.js`, `backend/controllers/authController.js`, and a user model.
5. Fix GitHub Action workflow to point to the actual backend package location.

---

This file is intended as a basic implementation guide, not a complete technical spec. Use it to organize the missing pieces and build the app in phases. Happy Coding ^^