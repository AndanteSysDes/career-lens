---
document: CareerLens Portfolio — Architecture Design
version: 0.1.0
status: draft
scope: Phase 1
last_updated: 2026-11-24
language: en
source_requirements: ../requirements/phase1.md
---

# CareerLens Portfolio — Architecture Design (Phase 1)

**Status:** In progress. Only decisions explicitly marked **Approved** below are binding. Other topics remain open for basic design.

## 1. Purpose and Reference

This document translates the approved [Phase 1 Requirements Specification](../requirements/phase1.md) into an implementable architecture. Implementation will be carried out incrementally using the project's agreed AI-assisted development guidelines.

## 2. Confirmed Architecture from Requirements

| Concern | Service / Technology |
| --- | --- |
| Frontend application | Next.js + TypeScript |
| Frontend hosting | Vercel |
| Backend REST API | Java + Spring Boot |
| Backend hosting | Azure Container Apps |
| Relational database | Supabase PostgreSQL Free |
| Contact email delivery | Resend |
| Static screenshots | Next.js `public/`, delivered by Vercel |
| CI/CD | GitHub Actions for the API; Vercel Git integration for the frontend |

Application runtime database access is **exclusively through the Spring Boot REST API**. Next.js does not connect directly to PostgreSQL. Supabase Auth and managed media storage are outside Phase 1.

## 3. Architecture Decision Log

### AD-001 — Server-side retrieval of public content (Approved)

**Decision date:** 2026-11-24

**Decision:** Retrieve public Profile, Skills, Projects, and Project Detail data from the Java REST API **on the Next.js server**, not via direct browser-to-Java API requests.

**Primary data flow:**

```text
Visitor's browser
       |
       | HTTPS page request
       v
Next.js server / Vercel
       |
       | HTTPS REST request (server-to-server)
       v
Spring Boot API / Azure Container Apps
       |
       | Database connection
       v
Supabase PostgreSQL
```

Next.js renders public pages using server-side data and returns the rendered page to the visitor. Server Components may be used where appropriate. Public content is cached using **ISR or an equivalent supported Next.js/Vercel strategy** as required by the approved specification.

**Rationale:**
- Enable search engines and visitors to receive meaningful initial page content.
- Reduce direct dependence of a visitor's browser on the Azure API.
- Support caching to reduce API/database traffic and mitigate some temporary downstream failures.
- Keep business logic and application data access in the Java backend.

**Boundaries and limitations:**
- No direct browser calls to the Java API are planned for *public content retrieval*. Client-side interactions can still be used for UI behavior.
- Client-side versus API-side filtering is **not yet decided**.
- Contact-form submission is a separate write operation; its exact route is **not yet decided** and must not use public-page caching.
- ISR does not guarantee availability for requests without a valid cache.
- Revalidation intervals, cache keys, invalidation rules, error handling, and runtime rendering details remain **to be designed**.

## 4. Open Basic-design Topics

- Rendering and caching details for dynamic route pages.
- Project list filtering execution location and associated API behavior.
- Contact-form submission path and abuse controls.
- Java backend application layering and dependency choices.
- Database physical schema, constraints, and migration strategy.
- REST API endpoints, response envelopes, and error contracts.
- Availability, monitoring, and deployment configuration.

## 5. Change Policy

Record each newly approved design choice in this document (or a linked design document), including its rationale. Do not treat an undecided item as approved. Keep implementation work scoped to small, reviewable changes with tests.
