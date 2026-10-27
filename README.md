# RetinaX Model Client - Web Interface for Verifiable AI Audit Trails on Stellar

[![Next.js](https://img.shields.io/badge/Next.js-15-black.svg)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-blue.svg)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue.svg)](https://www.typescriptlang.org/)
[![Soroban](https://img.shields.io/badge/Built%20on-Stellar%20Soroban-blue.svg)](https://soroban.stellar.org/)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![CI](https://github.com/RetinaX-Org/RetinaX-model-client/actions/workflows/ci.yml/badge.svg)](https://github.com/RetinaX-Org/RetinaX-model-client/actions)

RetinaX Model Client is the Next.js 15 (App Router) frontend for the [RetinaX Model](https://github.com/RetinaX-Org/retinaX-model) protocol. It is the public story of the protocol, the documentation hub, and the home of the operator dashboards and audit exports for verifiable AI billing on Stellar. The Soroban contracts hold the rules; this app is the human interface to them. Built for enterprises, inference providers, auditors and contributors who need to understand and verify what gets attested on-chain.

The client never holds secrets. RPC URLs, signing keys and vendor tokens stay in [RetinaX Model Backend](https://github.com/RetinaX-Org/RetinaX-model-backend), and the browser only receives `NEXT_PUBLIC_*` configuration.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Setup Instructions](#setup-instructions)
  - [Prerequisites](#prerequisites)
  - [Quick Start](#quick-start)
  - [Environment Variables](#environment-variables)
  - [Running Tests](#running-tests)
  - [Scripts](#scripts)
- [Features](#features)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Routes](#routes)
- [Integration Contract](#integration-contract)
- [Deployment](#deployment)
- [CI/CD Pipeline](#cicd-pipeline)
- [Helpful Links](#helpful-links)
- [Contribution Guidelines](#contribution-guidelines)
- [License](#license)
- [Support](#support)

---

## Project Overview

AI procurement is scaling faster than governance. Teams cannot consistently prove model version, region or policy for a given output, and usage-based billing for inference lacks a shared neutral layer, which leads to disputes between buyers and providers. RetinaX Model records attestations, metering and settlement on Soroban. This client makes that record legible to the people who rely on it.

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Trust Is Narrative + Proof** | Reviewers and security teams start on the website, not in the contracts |
| **Secrets Stay Server-Side** | RPC and signing secrets never ship in the browser bundle |
| **Delivery Transparency** | The home page publishes which routes are shipped, scaffolded or planned |
| **Accessible by Default** | Landmarks, skip links and axe checks run in the end-to-end suite |
| **Performance Budgeted** | CI fails when shared JS chunks exceed the gzipped budget |

### Target Users

- Enterprises and procurement teams needing audit exports
- Inference vendors and gateway operators
- Independent auditors and compliance teams
- Open-source contributors

---

## Setup Instructions

### Prerequisites

- **Node.js 20.x or 22.x** (LTS) - [Install Node.js](https://nodejs.org/)
- **npm** - bundled with Node.js
- **Git** - for version control

### Quick Start

```bash
# Clone the repository
git clone https://github.com/RetinaX-Org/RetinaX-model-client.git
cd RetinaX-model-client

# Install dependencies
npm ci

# Start the dev server with hot reload
npm run dev
```

Open **http://localhost:3000**. The static scaffold needs no secrets.

To run against the API, start [RetinaX Model Backend](https://github.com/RetinaX-Org/RetinaX-model-backend) separately and set its `CORS_ORIGIN` to this app's origin (for example `http://localhost:3000`).

### Environment Variables

Use `.env.local` (gitignored). Only browser-safe `NEXT_PUBLIC_*` variables belong here. Never put private keys or RPC URLs in them.

| Variable | Example | Purpose |
|----------|---------|---------|
| `NEXT_PUBLIC_STELLAR_NETWORK` | `testnet` / `public` | Network label shown in the UI |
| `NEXT_PUBLIC_APP_URL` | `https://...` | Canonical URL for OG tags and redirects |
| `NEXT_PUBLIC_BACKEND_URL` | `http://localhost:8080` | Browser-safe pointer to the API |
| `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` | `retinax-model.example` | Enables cookieless analytics and Web Vitals reporting. Unset makes both no-ops |
| `NEXT_PUBLIC_DEPLOY_ENV` | `preview` | Set by the preview workflow; pins the build to testnet and marks it noindex |

### Running Tests

```bash
# Lint
npm run lint

# Unit and component tests (Vitest)
npm test

# End-to-end and accessibility tests (Playwright + axe)
npm run test:e2e
```

Fix all ESLint and TypeScript errors before opening a pull request.

### Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Dev server with hot reload |
| `npm run build` | Production build |
| `npm start` | Serve the production build |
| `npm run lint` | ESLint (`next/core-web-vitals`) |
| `npm test` | Vitest unit and component tests |
| `npm run test:watch` | Vitest in watch mode |
| `npm run test:e2e` | Playwright end-to-end tests |
| `npm run test:e2e:ui` | Playwright with the interactive UI |
| `npm run size` | size-limit performance budget (run after `npm run build`) |

---

## Features

- Global shell with primary navigation derived from a single site map
- Product, contracts, compliance, roadmap and contributor pages
- Documentation hub covering API, architecture, contracts, integration and security
- Operator dashboard preview with usage, quota, settlement and attestation panels
- Audit export sample (`public/retinax-model-audit-sample.csv`) linked from the compliance page
- Optional Freighter wallet connection for demos, using test keys only
- Cookieless Plausible analytics and Web Vitals reporting, off unless configured
- Preview deployments pinned to testnet, marked noindex, and verified at build time
- Accessibility landmarks, skip link and axe-checked routes

---

## Architecture

| Layer | Choice |
|-------|--------|
| Framework | Next.js 15, App Router, React 19 |
| Language | TypeScript (strict) |
| Styling | CSS variables in `app/globals.css` |
| Wallet | `@stellar/freighter-api` (demo flows only) |
| Data | Static scaffold today; Server Components and the backend API for authenticated flows |
| Unit tests | Vitest and Testing Library |
| End-to-end tests | Playwright and axe-core |
| Performance | size-limit budget on shared JS chunks |

---

## Project Structure

```
RetinaX-model-client/
│
├── app/                        # App Router routes
│   ├── layout.tsx              # Shell: metadata and navigation
│   ├── page.tsx                # Landing page and delivery table
│   ├── product/ contracts/ operators/ compliance/ roadmap/ contributors/
│   ├── docs/                   # Documentation hub (api, architecture, contracts, integration, security)
│   ├── explore/                # Explorer with server actions
│   └── globals.css             # Design tokens and theme
│
├── components/                 # Shared UI (nav, footer, charts, forms, wallet, analytics)
├── lib/                        # Site map, deploy-env guard, analytics, API client, rate limiting
├── public/                     # Static assets and the audit export sample
├── tests/unit/                 # Vitest unit and component tests
├── e2e/                        # Playwright end-to-end and accessibility tests
│
├── .github/workflows/          # CI, preview deploy and preview cleanup
├── .size-limit.json            # Performance budget
├── next.config.ts              # Next.js configuration
└── README.md                   # This file
```

---

## Routes

`lib/site-map.ts` is the single source of truth for routes, purpose and delivery status. The header navigation, the delivery table on `/` and the end-to-end coverage all derive from it.

| Route | Purpose | Status |
|-------|---------|--------|
| `/` | Marketing hub and site map | Scaffold |
| `/product` | Personas, pricing hooks, integration story | Planned |
| `/contracts` | Soroban modules and interaction flows | Planned |
| `/operators` | Dashboard preview for AI gateways | Planned |
| `/compliance` | Audit exports and policy packs | Planned |
| `/roadmap` | Milestones | Scaffold |
| `/contributors` | Good first issues and roles | Planned |
| `/docs` | Technical reference hub | Scaffold |

Statuses here may lag the code; check `lib/site-map.ts` for the current values.

---

## Integration Contract

- **REST**: call [RetinaX Model Backend](https://github.com/RetinaX-Org/RetinaX-model-backend) under `/api/v1/*` from Route Handlers or authenticated clients. Never ship server secrets in `NEXT_PUBLIC_*`.
- **Soroban**: demonstrate wallet flows with test keys only. Production signing belongs in the backend or a secure wallet.
- **Contracts**: rules live in [RetinaX Model](https://github.com/RetinaX-Org/retinaX-model). The UI reflects state through Horizon, indexers or the backend.

---

## Deployment

Any Next.js host works (Vercel, Netlify, Cloudflare Pages).

1. Build command: `npm run build`
2. Output: Next.js default (`.next`)
3. Configure `NEXT_PUBLIC_*` variables per environment
4. Point `NEXT_PUBLIC_BACKEND_URL` at the deployed API
5. Enable preview deployments for demo links

### Preview Deployments

`.github/workflows/preview.yml` deploys a preview and comments the URL on every pull request. It needs four repository secrets:

| Secret | Where to find it |
|--------|------------------|
| `VERCEL_TOKEN` | Vercel account settings, Tokens |
| `VERCEL_ORG_ID` | `.vercel/project.json` after `vercel link` |
| `VERCEL_PROJECT_ID` | `.vercel/project.json` after `vercel link` |
| `STAGING_BACKEND_URL` | Your staging API origin; previews are pinned to it |

The workflow runs on `pull_request`, not `pull_request_target`, so a fork cannot reach these secrets. Fork runs skip the deploy and report a notice instead of failing.

Previews build with `NEXT_PUBLIC_DEPLOY_ENV=preview`, which pins them to testnet, shows a preview badge, and serves `Disallow: /` plus an `X-Robots-Tag: noindex` header. `lib/deploy-env.ts` re-checks these targets during the build and fails it if a preview is configured to reach production.

---

## CI/CD Pipeline

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `ci.yml` | Push/PR | Lint, build, unit tests, e2e and the size budget |
| `preview.yml` | Pull request | Deploy a preview, post the URL and Lighthouse scores |
| `preview-cleanup.yml` | PR merged or closed | Remove that pull request's deployments |

### Performance Budget

`npm run size` checks the gzipped weight of every cross-route JS chunk (`.next/static/chunks/*.js`) against `.size-limit.json`, currently 300 KB. It sums every shared chunk rather than de-duplicating per page, so it is a separate metric from the "First Load JS" figure `next build` prints. CI runs it after `npm run build` and fails on a regression.

---

## Helpful Links

### Repositories

- [RetinaX Model](https://github.com/RetinaX-Org/retinaX-model) - on-chain audit protocol and contracts
- [RetinaX Model Backend](https://github.com/RetinaX-Org/RetinaX-model-backend) - trusted API edge
- [RetinaX](https://github.com/RetinaX-Org/RetinaX) - decentralized vision care records

### Repository Resources

- [Contributing Guide](CONTRIBUTING.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Security Policy](SECURITY.md)

### External Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Stellar Developer Docs](https://developers.stellar.org/)
- [Soroban Documentation](https://soroban.stellar.org/docs)

---

## Contribution Guidelines

We welcome contributions. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

1. **Fork** the repository and clone your fork
2. **Create** a feature branch: `git checkout -b feature/issue-number-description`
3. **Make** your changes and keep route changes in `lib/site-map.ts`
4. **Test**: `npm run lint && npm test && npm run build && npm run size`
5. **Commit** with a conventional message: `feat: description (closes #issue-number)`
6. **Push** and open a pull request using the template

CI must pass and at least one review is required before merging.

---

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

---

## Support

- **Issues**: [GitHub Issues](https://github.com/RetinaX-Org/RetinaX-model-client/issues)
- **Security**: see [SECURITY.md](SECURITY.md) for private reporting

---

<p align="center">
  Built with ❤️ on Stellar
</p>
