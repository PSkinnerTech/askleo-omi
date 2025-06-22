# Getting Started with Askleo (Vite + SPA Edition)

This guide will walk you through setting up the Askleo project for local development, based on the new Vite + React + Fastify architecture.

**Package Manager**: This project uses `bun` exclusively. Please do not use `npm`, `pnpm`, or `yarn`.

## 1. Prerequisites

Before you begin, ensure you have the following installed:
-   [Node.js](https://nodejs.org/) (v20 or higher)
-   [Bun](https://bun.sh/) (v1.0 or higher)
-   An active [OpenAI API key](https://platform.openai.com/api-keys)
-   A [Supabase account](https://supabase.com/)

## 2. Initial Setup

### Step 2.1: Clone the Repository
First, clone the project repository to your local machine.

```bash
git clone https://github.com/PSkinnerTech/gai-askleo.git
cd gai-askleo
```

### Step 2.2: Install Dependencies
Install all the necessary packages for the entire monorepo using `bun`.

```bash
bun install
```

## 3. Environment Variables

The project requires several environment variables to connect to external services. The new architecture splits these between the frontend and the backend.

### Step 3.1: Backend Environment (`apps/api/.env`)
Create a new file named `.env` in the `apps/api` directory.

```dotenv
# apps/api/.env

# Supabase Admin
# Get these from your Supabase project settings -> API
SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=

# Supabase JWT Secret
# Get this from your Supabase project settings -> API -> JWT Settings
SUPABASE_JWT_SECRET=

# OpenAI (AI Suggestions)
# Get this from your OpenAI dashboard
OPENAI_API_KEY=
```

### Step 3.2: Frontend Environment (`frontend/.env` or `src/.env`)
Create a new file named `.env` in your frontend application's root directory.

```dotenv
# frontend/.env

# Supabase Public Credentials
# Get these from your Supabase project settings -> API
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```
**Note**: Vite requires environment variables exposed to the browser to be prefixed with `VITE_`.

## 4. Third-Party Service Configuration

### Step 4.1: Supabase Setup
1.  Go to [Supabase](https://supabase.com/) and create a new project.
2.  Navigate to **Project Settings** > **API**.
3.  Copy the **Project URL** and the **`anon` `public` key** into your frontend `.env` file.
4.  Copy the **`service_role` `secret` key** and the same **Project URL** into your `apps/api/.env` file.
5.  Under the **API** settings, find the **JWT Settings** section and copy the **JWT Secret** into `apps/api/.env`.

### Step 4.2: OpenAI Setup
1.  Go to the [OpenAI Platform](https://platform.openai.com/api-keys) and create a new API key.
2.  Copy this key into the `OPENAI_API_KEY` variable in your `apps/api/.env` file.

## 5. Database Migration

The database schema is managed via Drizzle ORM from your local machine. Before running the application, you need to apply the migrations to your Supabase database.

First, ensure your `DATABASE_URL` is set correctly in a root `.env` file for Drizzle Kit to connect.

```dotenv
# ./.env (for Drizzle migrations only)
DATABASE_URL=postgres://... # Your Supabase connection string
```

Now, run the migration:
```bash
bun run db:migrate
```
This command uses Drizzle Kit to apply all SQL migration files from the `supabase/migrations` directory to your database.

## 6. Running the Application

The Askleo application runs as two separate processes: the frontend dev server and the backend API server. You will need to run them in two separate terminal windows.

### Terminal 1: Start the Backend API Server

```bash
# Navigate to the API directory
cd apps/api

# Start the Fastify server
bun run dev
```
The API will now be running, typically on a port like `3001`.

### Terminal 2: Start the Frontend Development Server

```bash
# Navigate to the frontend directory (e.g., /src or /frontend)
cd src # or cd frontend

# Start the Vite dev server
bun run dev
```
The frontend application will now be available, typically at `http://localhost:5173`.

You have successfully set up the Askleo project for local development!
