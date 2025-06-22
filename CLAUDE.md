# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Askleo is an AI-powered medical documentation assistant designed to help healthcare providers create accurate SOAP notes through real-time AI suggestions and medical terminology validation.

## Development Commands

### Root Directory
```bash
npm run dev      # Start development server (http://localhost:5173)
npm run build    # Build for production
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

### Frontend Directory (`/frontend`)
```bash
npm run dev      # Start development server
npm run build    # Build for production
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

## Architecture

The project uses a monorepo structure with duplicate React/Vite setups at root and in `/frontend`. The active development should happen in the `/frontend` directory.

### Frontend Architecture
- **Framework**: React 19.1 + TypeScript + Vite
- **Routing**: react-router-dom for client-side navigation
- **State Management**: React Context for auth, @tanstack/react-query for server state
- **Styling**: Tailwind CSS
- **Component Structure**:
  - `components/` - Reusable UI components
  - `pages/` - Route-specific page components
  - `hooks/` - Custom React hooks
  - `services/` - API and external service integrations

### Editor Design
The medical documentation editor uses a three-panel layout:
1. Left sidebar for navigation and templates
2. Center editor with plain text state and HTML rendering
3. Right panel for AI suggestions

Key implementation details:
- Plain text internal state with HTML rendering for display
- Highlight-based suggestion system for precise text replacement
- WebSocket connections for real-time AI analysis
- Text replacement using exact string matching

### Backend Architecture (Planned)
- **Platform**: Supabase Edge Functions
- **Authentication**: Supabase Auth with JWT tokens
- **Database**: Supabase (PostgreSQL)
- **AI Integration**: OpenAI GPT-4.1 API

## Key Implementation Notes

1. **Text Replacement Logic**: The editor uses a highlight-based system where AI suggestions map to specific text ranges. Replacements must preserve exact character positions.

2. **Authentication Flow**: All API calls will require Supabase JWT tokens passed as Bearer tokens in headers.

3. **Real-time Features**: WebSocket connections handle live AI analysis as users type, with debouncing to prevent excessive API calls.

4. **SOAP Note Structure**: The application enforces standard SOAP format (Subjective, Objective, Assessment, Plan) with medical terminology validation.

## Current Development Status

The project is in early setup phase. Core features pending implementation:
- Editor component with suggestion system
- Supabase backend integration
- Authentication system
- Document management features
- AI analysis integration