# Askleo - Development Checklist

This document provides a comprehensive, step-by-step checklist for building the Askleo application from scratch, based on the architectural and feature documentation.

---

## Phase 1: Project Setup and Foundation

-   [ ] **1.1. Initialize Monorepo Structure**:
    -   [ ] Create the root project directory.
    -   [x] Create the `frontend` directory for the Vite + React app.
    -   [ ] Create the `supabase/functions` directory for the backend API.

-   [ ] **1.2. Set Up External Services**:
    -   [ ] Create a new project in Supabase.
    -   [ ] Create a new project in OpenAI to get an API key.

-   [ ] **1.3. Environment Variable Configuration**:
    -   [ ] Create the `.env` file in the `frontend` directory with `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`.
    -   [ ] Create the `.env` file in the `supabase/functions` directory with `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, `SUPABASE_JWT_SECRET`, and `OPENAI_API_KEY`.

-   [ ] **1.4. Database Schema and Migrations**:
    -   [ ] Define all Drizzle schemas (`profiles`, `documents`, `suggestions`, etc.) in the `db/schema` directory.
    -   [ ] Ensure the `profiles` schema has a foreign key to `auth.users(id)`.
    -   [ ] Generate the initial database migration using `bun run db:generate`.
    -   [ ] Apply the migration to the Supabase database using `bun run db:migrate`.

---

## Phase 2: Authentication and Core Layout

-   [ ] **2.1. Implement Supabase Authentication**:
    -   [ ] Create Supabase Edge Functions for `sign-up`, `sign-in`, and `sign-out`.
    -   [ ] Build the frontend UI components for the login, sign-up, and password reset pages.
    -   [ ] Create a global `AuthProvider` context in the React app to manage session state and listen to `onAuthStateChange`.

-   [ ] **2.2. Set Up Client-Side Routing**:
    -   [ ] Install `react-router-dom`.
    -   [ ] Define all application routes (`/`, `/login`, `/document/:id`, `/settings`, etc.) in `App.tsx`.
    -   [ ] Create a `ProtectedRoute` component that checks the auth state and redirects unauthenticated users to `/login`.

-   [ ] **2.3. Build the Main Application Layout**:
    -   [ ] Create the main three-panel, resizable layout component.
    -   [ ] Implement the `DocumentSidebar` component to display a list of documents.
    -   [ ] Implement the `TopNav` component with user profile and navigation controls.
    -   [ ] Implement the `AISuggestionsPanel` component as a placeholder.

---

## Phase 3: Core Editor and Text Replacement Logic

-   [ ] **3.1. Implement the Core Editor**:
    -   [ ] Build the `EnhancedEditor` component using a `contentEditable` div.
    -   [ ] Implement the "Plain Text vs. Rendered HTML" architecture to separate state from the view.
    -   [ ] Implement the `renderHighlightedHTML` function to display highlights.

-   [ ] **3.2. Implement Backend Analysis Function**:
    -   [ ] Create the `/analyze-text` Supabase Edge Function.
    -   [ ] Implement the `findTextSpan` logic within the function to precisely locate errors.
    -   [ ] Integrate the OpenAI API call to get suggestions with `context`.
    -   [ ] Implement the Drizzle query to save new suggestions to the database.

-   [ ] **3.3. Implement Frontend Suggestion Handling**:
    -   [ ] Implement the `applySuggestion` function in the editor to handle local text replacement.
    -   [ ] After local replacement, trigger a `fetch` call to an `/apply-suggestion` Edge Function to update the `isAccepted` flag in the database.
    -   [ ] Implement the suggestion priority system (`deduplicateHighlights`) to resolve overlapping suggestions.
    -   [ ] Implement the logic to dynamically adjust all other highlight positions after an edit is made.

-   [ ] **3.4. Implement Suggestion Persistence**:
    -   [ ] Create a `/get-suggestions/:documentId` Edge Function to fetch unaccepted suggestions from the database.
    -   [ ] On document load, call this endpoint and use the `findNewPosition` logic to re-validate and render persisted suggestions.

---

## Phase 4: Feature Implementation

-   [ ] **4.1. Document Management**:
    -   [ ] Implement Edge Functions for creating, updating, and deleting documents.
    -   [ ] Connect the `DocumentSidebar` to fetch and display the user's documents.
    -   [ ] Implement auto-save and version history functionality.

-   [ ] **4.2. Medical Terminology and SOAP Formatting**:
    -   [ ] Integrate medical dictionary libraries into the `/analyze-text` function.
    -   [ ] Update the AI prompt to include rules for SOAP note structure validation.
    -   [ ] Create UI components or templates in the frontend to guide users in writing SOAP notes.

-   [ ] **4.3. User Profile and Settings**:
    -   [ ] Build the `/profile` and `/settings` pages.
    -   [ ] Implement Edge Functions to fetch and update user preferences.

---

## Phase 5: Deployment and Finalization

-   [ ] **5.1. Prepare for Deployment**:
    -   [ ] Configure the `frontend` and `supabase/functions` for a production build.
    -   [ ] Set up a new project on Fly.io.

-   [ ] **5.2. Deploy a Staging Environment**:
    -   [ ] Deploy the Fastify API to a staging environment on Fly.io.
    -   [ ] Deploy the Vite frontend to a staging environment.
    -   [ ] Configure all production environment variables.

-   [ ] **5.3. Final Testing**:
    -   [ ] Conduct end-to-end testing in the staging environment.
    -   [ ] Perform user acceptance testing (UAT).

-   [ ] **5.4. Production Deployment**:
    -   [ ] Deploy the application to the final production environment on Fly.io.

---
This checklist provides a logical path from a blank slate to a fully functional and deployed application, ensuring all documented architectural decisions and features are implemented. 