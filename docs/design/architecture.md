---
document: CareerLens Portfolio — Architecture Design
version: 0.2.0
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
- The project list is obtained server-side and filtered in the browser according to **AD-002**.
- Contact-form submission is a separate write operation; its exact route is **not yet decided** and must not use public-page caching.
- ISR does not guarantee availability for requests without a valid cache.
- Revalidation intervals, cache keys, invalidation rules, error handling, and runtime rendering details remain **to be designed**.

### AD-002 — Client-side filtering of the project list (Approved)

**Decision date:** 2026-11-24

**Decision:** Fetch the list of published projects from the Spring Boot API **on the Next.js server** (AD-001). Send the retrieved list to an interactive client-side component, which performs parent-category and subcategory filtering **in the visitor's browser**, without sending further requests for each filter change.

**Interaction flow:**

```text
Initial page request
  Next.js server -> Spring Boot API -> PostgreSQL
          |
          v
  Rendered project list + published project data
          |
          v
  Browser: change category/subcategory
          |
          v
  Filter already-received data locally (no new API request)
```

**Filtering rules (from the approved requirements):**
- Parent category: one of `All Projects`, `Professional`, or `Personal Project` (default `All Projects`).
- Show the subcategory selector only after a specific parent is selected.
- Subcategory: one of `All` or exactly one subcategory belonging to the selected parent (default `All`).
- Changing the parent resets the subcategory to `All`; old choices are not restored.
- Preserve the default order: `Professional` before `Personal Project`, newest first within each parent.
- Never include unpublished projects in the data delivered to the browser.

**Rationale:** For the expected size of a personal portfolio, local filtering is responsive, straightforward to test, and avoids repeated API calls and additional loading states.

**Boundaries and limitations:**
- This decision is specific to the public project list. Project detail, profile, and skills retrieval remain subject to AD-001.
- Query parameters, URL persistence of active filters, pagination, and exact handling of partially specified dates are **not decided** here.
- If the dataset grows significantly, the filtering strategy can be reassessed by a separately approved change.

## 4. Open Basic-design Topics

- Rendering and caching details for dynamic route pages.
- Contact-form submission path and abuse controls.
- Java backend application layering and dependency choices.
- Database physical schema, constraints, and migration strategy.
- REST API endpoints, response envelopes, and error contracts.
- Availability, monitoring, and deployment configuration.

## 5. Change Policy

Record each newly approved design choice in this document (or a linked design document), including its rationale. Do not treat an undecided item as approved. Keep implementation work scoped to small, reviewable changes with tests.
