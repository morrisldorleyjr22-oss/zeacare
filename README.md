# Zeal Care Liberia

Zeal Care Liberia is a full-stack nonprofit website and content-management platform focused on youth education, empowerment, leadership, and community development in Liberia.

The system combines a multilingual public website with an authenticated administration portal for managing beneficiaries, donations, contact messages, newsletter subscriptions, media, and organizational content.

## Key Features

### Public Website

- Organization profile, mission, vision, values, and history
- Education, mentorship, technology, leadership, and community programs
- Beneficiary stories and sponsorship information
- Donation interest and contact workflows
- News, media, events, galleries, and partner information
- Newsletter subscription
- Responsive navigation and mobile layouts
- English, French, and Arabic localization support

### Administration Portal

- Secure administrator authentication with JSON Web Tokens
- Operational dashboard and impact statistics
- Beneficiary record management
- Donation record review and CSV export
- Contact-message management
- Newsletter subscriber management and CSV export
- Website content management
- Media uploads
- SMTP configuration and notification testing

## Technology Stack

| Layer | Technology |
| --- | --- |
| Monorepo | pnpm workspaces |
| Frontend | React, TypeScript, Vite |
| Interface | Tailwind CSS, Radix UI |
| Routing | Wouter |
| Data fetching | TanStack Query |
| Localization | i18next, React i18next |
| API | Express 5, TypeScript |
| Validation | Zod |
| Authentication | JSON Web Tokens |
| Persistence | Server-side JSON data stores |
| Email | Nodemailer with SMTP |
| Build tooling | TypeScript, esbuild |

## Repository Structure

```text
artifacts/
  zeal-care/           Public website and administration interface
  api-server/          Express API and server-side data stores
lib/
  api-client-react/    Generated React API client
  api-spec/            OpenAPI specification
  api-zod/             Generated validation schemas
  db/                  Shared database package
scripts/               Build and maintenance scripts
api/                   Serverless API entry point
public/                Shared public media
vercel.json            Production build configuration
```

## Prerequisites

- Node.js 24
- pnpm

Enable pnpm through Corepack if it is not already available:

```bash
corepack enable
corepack prepare pnpm@latest --activate
```

## Local Setup

1. Clone the repository and switch to its active branch.

   ```bash
   git clone https://github.com/morrisldorleyjr22-oss/zeacare.git
   cd zeacare
   git switch replit-agent
   ```

2. Install workspace dependencies.

   ```bash
   pnpm install
   ```

3. Configure the server environment.

   ```env
   ADMIN_PASSWORD=use-a-strong-unique-password
   ADMIN_JWT_SECRET=use-a-long-random-secret
   SMTP_HOST=smtp.example.com
   SMTP_PORT=587
   SMTP_USER=notifications@example.com
   SMTP_PASS=your-smtp-password
   NOTIFY_EMAIL=operations@example.com
   ```

4. Start the API server.

   ```bash
   pnpm --filter @workspace/api-server run dev
   ```

5. Start the website in another terminal.

   ```bash
   pnpm --filter @workspace/zeal-care run dev
   ```

## Available Commands

| Command | Purpose |
| --- | --- |
| `pnpm run typecheck` | Type-check the complete workspace |
| `pnpm run build` | Type-check and build all supported packages |
| `pnpm --filter @workspace/zeal-care run dev` | Start the frontend development server |
| `pnpm --filter @workspace/zeal-care run build` | Build the frontend |
| `pnpm --filter @workspace/api-server run dev` | Build and start the API server |
| `pnpm --filter @workspace/api-server run build` | Build the API server |

## API Overview

Public endpoints provide health checks, site content, contact submission, newsletter subscription, beneficiary information, and donation workflows. Protected administrator endpoints manage content, beneficiaries, donations, messages, subscribers, uploads, and email settings.

All API routes use the `/api` prefix. The API specification is maintained in `lib/api-spec/openapi.yaml`.

## Data Storage

The API currently stores operational data in server-side JSON files. The deployment environment must provide persistent storage for the configured data directory. Ephemeral serverless storage can cause records and uploads to disappear between deployments or function restarts.

Use a managed database and durable object storage before handling production-scale or sensitive records.

## Security Requirements

- Set unique production values for `ADMIN_PASSWORD` and `ADMIN_JWT_SECRET`.
- Never commit credentials, SMTP passwords, tokens, or private donor information.
- Restrict administration routes to trusted origins.
- Back up beneficiary, donation, message, and subscriber data.
- Review file-upload validation and storage controls before production use.
- Treat beneficiary details and donor information as sensitive data.

## Quality Checks

Run these checks before deployment:

```bash
pnpm run typecheck
pnpm run build
```

Verify the public website, administration login, content editing, contact form, newsletter registration, donation workflow, uploads, and email notifications in the target environment.

## Deployment

The repository includes Vercel build configuration. Production deployments must provide all required environment variables and durable storage for server-managed data and uploads.

## Maintainer

Developed and maintained by [Morris L. Dorley Jr.](https://morris.innova-lib.com) through [Innova Liberia](https://innova-lib.com).
