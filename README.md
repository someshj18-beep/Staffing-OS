# StaffingOS

**The Operating System for Modern Staffing Agencies.**

This repository is a deployable Next.js + Supabase foundation based on the StaffingOS master build specification.

## What is implemented in this foundation

- Enterprise SaaS shell and responsive navigation
- Agency dashboard with executive KPIs
- Companies / client CRM list
- Candidate database
- Candidate 360° profile
- Jobs & requirements
- Recruitment Kanban
- AI Candidate Finder UI with provider-ready boundary
- Agency analytics
- PostgreSQL schema for multi-tenancy
- Supabase RLS organization isolation
- Core ATS/CRM/finance relationships
- GitHub → Vercel deployment structure

The master specification explicitly requires real actions rather than fake functionality. Therefore integrations such as AI providers, Gmail/Microsoft, WhatsApp Business, invoicing delivery and payments are intentionally configuration points rather than disguised mock integrations.

## Deploy

1. Create a Supabase project.
2. Open Supabase SQL Editor and run `supabase/migrations/001_staffingos.sql`.
3. Create the private Storage buckets described in the SQL comments.
4. Create a GitHub repository and push this folder.
5. Import the repository into Vercel.
6. Add `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in Vercel.
7. Deploy.

## Local development

A local Node environment is not required for deployment. If you do want to develop locally, install dependencies with your package manager and run `npm run dev`.

## Next implementation layers

The schema is intentionally designed so these modules can be added without replacing the foundation:

- Auth/onboarding and organization creation
- Full CRUD server actions for every module
- Resume upload + parser provider
- pgvector embeddings and semantic matching
- Gmail / Microsoft 365 / SMTP
- Official WhatsApp Business API
- Automation engine
- Contract staffing + timesheets
- Invoice PDF generation and payment tracking
- Commission engine
- Client portal and candidate portal
- KPI/KRA calculation engine
- SaaS subscription/usage enforcement

\n## Live data\n\nCompanies and Candidates now perform real Supabase reads/inserts for the signed-in user's organization. RLS remains the isolation boundary. AI, email, WhatsApp and payments are provider/configuration dependent and are not simulated.\n

## Authentication and live jobs

1. Run migrations in order: `001_staffingos.sql`, then `003_workflow_security.sql`.
2. Set `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in Vercel.
3. Enable Email/Password in Supabase Auth and configure the deployed domain as an allowed redirect URL.
4. Visit `/login`, create an account, confirm email if required, then create the agency at `/onboarding`.
5. Companies, Candidates and Jobs use Supabase reads/inserts and require the signed-in user to have a `profiles` row linked to an organization.

AI, email, WhatsApp and payment integrations are not simulated; they require provider setup and their own implementation.
