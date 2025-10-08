# Overview

This is a DialFire campaign analytics dashboard application built with a modern full-stack architecture. The system monitors and analyzes call center performance metrics, tracking callers, teams, and campaigns through real-time data visualization and threshold-based alerting. The application integrates with the DialFire API to sync call data and provides comprehensive performance analytics across multiple dimensions including individual caller statistics, team performance, and campaign-wide metrics.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend Architecture

**Framework & Tooling**
- React 18 with TypeScript for type-safe component development
- Vite as the build tool and development server for fast HMR (Hot Module Replacement)
- Wouter for lightweight client-side routing
- TanStack Query (React Query) for server state management with automatic caching and refetching

**UI Component System**
- Radix UI primitives for accessible, unstyled component foundations
- shadcn/ui component library (New York style variant) for pre-built, customizable components
- Tailwind CSS for utility-first styling with CSS custom properties for theming
- Chart.js for data visualization (calls over time, team distribution, performance comparisons)

**State Management Pattern**
- Server state managed through React Query with configurable refetch intervals (30-60 seconds for real-time updates)
- Local UI state handled via React hooks
- Form state managed with React Hook Form and Zod validation

## Backend Architecture

**Server Framework**
- Express.js with TypeScript running on Node.js
- ESM (ES Modules) module system throughout the application
- Custom middleware for request logging and JSON response capture
- Vite integration for development with HMR support

**API Design**
- RESTful API endpoints organized by resource (dashboard, callers, teams, alerts, thresholds)
- Campaign-scoped queries with optional date range filtering
- Centralized error handling with descriptive error messages
- Response logging middleware for debugging and monitoring

**Storage Layer**
- Drizzle ORM for type-safe database operations
- PostgreSQL as the primary database (via Neon serverless driver)
- Schema-first design with Zod validation derived from Drizzle schemas
- Interface-based storage abstraction (IStorage) for potential database swapping

## Data Model

**Core Entities**
- **Campaigns**: Top-level organization unit with DialFire integration via dialfireId
- **Teams**: Grouped by contact owner ID for DialFire mapping (Assassins, Warriors, Titans)
- **Callers**: Individual agents linked to teams and DialFire via dialfireId
- **Calls**: Transaction records with duration, success status, and timestamps
- **Thresholds**: Campaign-specific performance benchmarks (min daily calls, success rate, avg duration, weekly targets)
- **Alerts**: Generated when callers fall below threshold metrics

**Analytics Models**
- CallerStats: Aggregated metrics per caller (total calls, success rate, avg duration, trend)
- TeamStats: Aggregated metrics per team with similar structure
- DashboardStats: Campaign-wide overview with totals and averages

## External Dependencies

**Third-Party Services**
- **DialFire API**: External call center platform integration for syncing call data, caller information, and campaign details. Requires API token authentication.
- **Neon Database**: Serverless PostgreSQL hosting for production data storage

**Key Libraries**
- `drizzle-orm` & `drizzle-kit`: Database ORM and migration tooling
- `@neondatabase/serverless`: PostgreSQL connection driver optimized for serverless environments
- `@tanstack/react-query`: Asynchronous state management
- `react-hook-form` & `@hookform/resolvers`: Form handling with Zod validation
- `chart.js`: Canvas-based charting library
- `date-fns`: Date manipulation and formatting
- `zod`: Runtime type validation and schema definition

**Development Tools**
- `tsx`: TypeScript execution for development server
- `esbuild`: Fast bundling for production builds
- Replit-specific plugins for development environment integration (cartographer, dev-banner, runtime-error-modal)

## Key Architectural Decisions

**Monorepo Structure**
- Shared schema definitions in `/shared` directory accessible to both client and server
- Path aliases configured (`@/` for client, `@shared/` for shared code)
- Single TypeScript configuration for consistent type checking

**DialFire Integration Strategy**
- Team mapping via contact owner IDs stored in database
- Default team mappings (Assassins: 1234, Warriors: 5678, Titans: 9012)
- Caller identification through DialFire agent IDs
- LocalStorage fallback for team mapping configuration

**Real-Time Updates**
- Stale-while-revalidate pattern with React Query
- Configurable refetch intervals based on data criticality (30s for alerts, 60s for statistics)
- Optimistic UI updates disabled (staleTime: Infinity) for data consistency

**Type Safety Approach**
- Drizzle schemas as single source of truth
- Zod schemas auto-generated from Drizzle for runtime validation
- Shared types between client and server via TypeScript inference
- Insert schemas for mutation operations

**Performance Optimization**
- Vite's code splitting for optimal bundle sizes
- React Query caching to minimize API calls
- Chart.js instance management to prevent memory leaks
- Mobile-responsive design with breakpoint-based rendering