---
document: CareerLens Portfolio — Phase 1 Requirements Specification
version: 1.0.0
status: approved
approved_scope: Phase 1
last_updated: 2026-11-24
language: en
frontend_hosting: Vercel
backend_hosting: Azure Container Apps
database: Supabase PostgreSQL Free
---

# CareerLens Portfolio — Requirements Specification

**Version:** 1.0.0  
**Status:** Approved for Phase 1 basic design  
**Last updated:** November 24, 2026  
**Primary language of this document:** English  
**Project:** CareerLens Portfolio

> **Scope of approval:** The functional and non-functional requirements for **Phase 1** are approved. Phase 2 and Phase 3 are included only as future-roadmap context; their detailed requirements are **not** approved by this document. Technical choices explicitly left for basic design are not implicitly approved.

## 1. Project Overview

### 1.1 Purpose

Create a professional portfolio website primarily for **employment and career opportunities**, enabling recruiters and hiring managers to quickly determine:

1. Whether the candidate may be suitable for a position.
2. Which professional and software-development experience the candidate has.
3. Which projects best demonstrate their capabilities.
4. How to contact the candidate if interested.

Phase 1 is **not** intended as a marketing or sales website for acquiring new freelance contracts. Past freelance engagements may be presented as portfolio experience.

### 1.2 Target Roles

- Java Backend Engineer
- Full-Stack Engineer

### 1.3 Target Audience

- Recruiters and talent acquisition specialists
- Engineering managers
- Technical interviewers
- Representatives of prospective employers

### 1.4 Capabilities to Communicate

- System and software architecture design
- Java and Spring Boot development
- Next.js and TypeScript development
- End-to-end ownership, from requirements through deployment
- Ability to learn and apply new technologies
- Ability to implement original ideas as working software
- Both professional and personal development experience

## 2. Existing Assets and Design Principles

### 2.1 Existing Application

The existing `Portfolio-Next.js` site is the content and visual-design starting point.

### 2.2 Migration Approach

Reuse relevant public information from the existing site while improving the information architecture so that employers can find and assess experience efficiently. This is **not** a requirement for a one-to-one migration.

### 2.3 Visual and Interaction Principles

- Retain and improve the existing visual style.
- Reuse suitable animations and interactive elements.
- Preserve and improve responsive layouts.
- Avoid excessive animation and decoration.
- Prioritize readability, navigation, and information discoverability.

## 3. Language and Localization Requirements

### 3.1 Supported Languages

- English (`en`)
- Japanese (`ja`)

English is the default language.

### 3.2 Language-specific URLs

- English pages use `/en/...`.
- Japanese pages use `/ja/...`.
- The root URL `/` always redirects to `/en`.
- Users can switch languages manually.
- The application **must not persist** language preference in cookies or local storage.
- Visiting a language-specific URL displays content in the language identified by that URL.

### 3.3 Translation Coverage

Both languages must be provided for:

- Navigation and page titles
- Section headings and explanatory text
- Profile content
- Skill-related UI and descriptive content
- Project information and full project-detail narratives
- Contact form labels and guidance
- Validation and error messages
- Buttons and calls to action (CTAs)

Project-detail content must be authored and maintained in both languages, including longer technical descriptions.

### 3.4 Terminology

Retain official technology names, product names, and proper nouns in their established form wherever appropriate (for example, `Java`, `Spring Boot`, `REST API`, `GitHub`). Translate the surrounding explanation rather than unnecessarily translating such names.

### 3.5 Content Storage

Language-independent attributes and translatable content are stored separately. Dedicated translation tables (for example, `Project` and `ProjectTranslation`) will be used. The logical model is defined in Section 11; physical schema details are deferred to basic design.

## 4. Phase 1 Site Structure

### 4.1 Pages in Scope

1. Home
2. About / Profile
3. Skills
4. Experience / Projects
5. Project Detail
6. Contact

### 4.2 Shared UI

All relevant pages should have:

- Header and navigation
- Language switcher
- Responsive layout
- Footer
- GitHub link
- Appropriate navigation and card interactions

### 4.3 Explicitly Out of Scope for Phase 1

- Contributions page
- Resume page
- AI chatbot
- Administration dashboard
- Administrator login and authentication
- Profile editing through a UI
- Project creation/editing/deletion through a UI
- Image upload/management through a UI
- Featured or pinned projects
- Manual ordering of projects

## 5. Home Page

### 5.1 Objective

Guide a visitor naturally from initial assessment to reviewing experience and, when appropriate, making contact.

```text
Assess suitability
        ↓
Review experience and projects
        ↓
Make contact
```

### 5.2 Hero Content

The first viewport includes:

- Professional headline
- Short introduction
- Representative technology stack
- Primary CTA to Experience
- Secondary CTA to Contact

**Headline:**

```text
Java Backend & Full-Stack Engineer
```

**Draft introductory copy** (subject to editorial refinement, not a final content freeze):

> I design and deliver reliable web systems, combining Java backend experience with full-stack development and hands-on ownership from requirements definition to deployment.

**CTA labels:**

- Primary: `View My Experience`
- Secondary: `Contact Me`

### 5.3 Additional Content

- Links to representative projects
- GitHub link
- Optional navigation to the profile

Decisions on avatar/photo, the number of highlighted projects, and precise technology-stack presentation are deferred to UI/content design.

## 6. About / Profile Page

### 6.1 Content

Show:

- Personal/professional profile
- Career summary
- Core areas of expertise
- Career strengths
- Development philosophy

### 6.2 Data and Editing

- Profile content is managed in PostgreSQL.
- Phase 1 provides public, read-only display.
- Initial content is populated through seed scripts, developer scripts, or migrations.
- UI-based editing is deferred to Phase 2.

## 7. Skills Page

### 7.1 Skill Categories

Candidate categories are:

- Backend
- Frontend
- Database
- Cloud / Infrastructure
- Testing
- Tools
- Other

### 7.2 Representative Technologies

- Java
- Spring Boot
- Next.js
- TypeScript

Other skill entries are drawn from public information in `Portfolio-Next.js`.

### 7.3 Data Management

- Skills are stored in PostgreSQL.
- Phase 1 displays them as public data via the backend API.
- The presentation of proficiency levels, years of experience, category grouping, and icons is left to UI/content design; these details are not mandatory Phase 1 features.

## 8. Experience / Projects Page

### 8.1 Objective

Present professional and personal development experience together so that employers can compare and explore projects efficiently. Both appear on the **same page**, not separate top-level pages.

### 8.2 Project Classification

Every project **must** have exactly one **parent category** and exactly one **subcategory** belonging to that parent. Multiple category or subcategory assignments are not permitted.

| Parent category | Meaning | Subcategory | Meaning |
|---|---|---|---|
| `Professional` | Paid professional work | `Employment` | Paid work performed under an employment relationship |
| `Professional` | Paid professional work | `Freelance` | Contract work undertaken as a freelancer |
| `Personal Project` | Unpaid personal or school development work | `School Assignment` | A project developed as a school assignment |
| `Personal Project` | Unpaid personal or school development work | `Personal Learning Project` | A project created for self-directed learning |
| `Personal Project` | Unpaid personal or school development work | `Independent Project` | An independently conceived project developed primarily for practical use rather than as coursework or a learning exercise |

`Independent Project` is an approved classification value even though no current project requires it. No additional special behavior is required for this subcategory in Phase 1.

The previous model, in which experience types were independently assigned as multiple tags, is superseded and **must not** be implemented.

### 8.3 Filters

**Parent-category filter:**

- Single-select.
- Options: `All Projects`, `Professional`, `Personal Project`.
- Initial value: `All Projects`.

**Subcategory filter:**

- Shown only when a specific parent category is selected.
- Single-select.
- Options: `All` plus only the subcategories belonging to the selected parent.
- Initial value: `All`.
- On every change of parent category, reset the selected subcategory to `All`.
- Do not restore a previously selected subcategory when returning to a parent category.

**Expected filter behavior:**

- `All Projects`: display all published projects from both parent categories.
- Specific parent + `All`: display all published projects in that parent category.
- Specific parent + specific subcategory: display only published projects belonging to that subcategory.

### 8.4 Default Ordering

- Display `Professional` projects before `Personal Project` projects.
- Within each parent category, display projects in descending chronological order (newest first).
- No featured-project prioritization, manual ordering, or pinning is required.

The exact date comparison for projects with equal or partially specified dates is a basic-design detail.

## 9. Project Detail Page

### 9.1 Objective

Explain what was built and what the candidate personally contributed.

### 9.2 Display Priority and Order

Display available sections in the following order:

1. Project overview
2. Personal role and responsibilities
3. Technology stack
4. System design / architecture
5. Development process
6. Challenges and solutions
7. Outcomes / results
8. Images / screenshots
9. External links

The **overview** and **personal role** are the highest-priority information.

### 9.3 Required Project Fields

The following are required for every project:

- Project name/title
- Summary/overview description
- Personal role
- Parent category
- Corresponding subcategory (required under the classification rules in Section 8)
- Start date or start period
- URL identifier (`slug`)
- Publication status

### 9.4 Optional Project Fields

- Architecture / system design
- Technology stack
- Development process
- Challenges
- Solutions
- Outcomes / results
- End date or period
- Screenshots
- External links

Omit empty optional sections from the public page. For any text field that is provided and is subject to localization, maintain both English and Japanese versions.

### 9.5 Markdown

- Store longer project narratives in Markdown.
- Store overview, role, architecture, development process, challenges, solutions, and results as **separate content fields**, not as a single unstructured document.
- Present the Markdown as formatted content.
- The exact rendering library and Markdown safety controls are selected during basic design.

### 9.6 Images in Phase 1

- Store screenshots and project images in the Next.js `public` directory.
- Serve them as static assets via Vercel.
- Add and update image files through Git.
- No image-upload UI is required.
- The logical data model should retain image reference information such as path, alternative text, caption, and display order.
- Do **not** store image binaries in PostgreSQL.
- Migration to Supabase Storage is deferred to Phase 2.

Exact image limits, external-link types, date display precision, and specific slug/URL patterns remain basic-design or UI/content decisions.

## 10. Contact Page

### 10.1 Purpose

Allow visitors to send recruitment-related inquiries to a designated recruitment email address.

### 10.2 Input Fields

- Sender name
- Sender email address
- Message body

The visitor does **not** enter the email subject.

### 10.3 Email Subject

Generate the subject on the server using:

```text
CareerLens Portfolio Contact from {senderName}
```

### 10.4 Submission and Delivery

- Do not store submitted inquiries in the application database.
- Send the message by email only.
- Do not display the recruitment mailbox address directly on the website.
- Use **Resend** as the email-delivery provider.
- The **Spring Boot API**, not the browser, invokes the Resend API.
- Store the Resend API key securely in Azure-side configuration/secrets; never expose it to the browser.
- Configure the actual verified sending domain and sender address during deployment.

### 10.5 Validation

At minimum, validate:

- Non-empty sender name
- Valid email-address format
- Non-empty message body
- Maximum message length
- Potentially malicious or unsafe input

Validation must be enforced server-side; client-side validation may additionally improve usability. Specific field-length limits and validation/error responses are basic-design decisions.

### 10.6 Spam Protection

Use **Honeypot + Rate Limiting** in Phase 1.

- Include a honeypot input intended to detect straightforward automated submissions.
- Validate the honeypot on the server.
- Apply request rate limiting in the Spring Boot API before attempting Resend delivery.
- Define and test exact thresholds and rate-limit keys during detailed design/implementation.
- Cloudflare Turnstile and other CAPTCHA-based solutions are **not** required for Phase 1; consider them only if operations demonstrate a need.

## 11. Data Management and Logical Model

### 11.1 Database

- Use **Supabase PostgreSQL Free** from Phase 1 onward.
- Store public profile, skill, and project data in PostgreSQL.
- Populate initial records through seeding scripts, developer scripts, and/or migrations.

### 11.2 Data Access Boundary

**Runtime data access must follow this route:**

```text
Visitor
  |
  v
Next.js on Vercel
  |
  | HTTPS / REST API
  v
Spring Boot on Azure Container Apps
  |
  | Database connection
  v
Supabase PostgreSQL
```

- Public reads for profile, skills, and projects go through the **Spring Boot REST API**.
- Next.js must not connect directly to the Supabase PostgreSQL database at runtime.
- Application database access and business logic are centralized in Spring Boot.
- Development/deployment-time database migrations and seeding are administrative operations outside the runtime restriction.
- A future Next.js integration with **Supabase Auth** is an explicitly permitted Phase 2 exception for authentication—not a Phase 1 direct-database access permission.

### 11.3 Logical Entities

The following attributes describe the **conceptual model**, not the final physical schema. Names, data types, keys, constraints, and migration scripts will be finalized in basic design.

**Profile**

- `id`
- `name`
- `location`
- `contact_visibility`
- `created_at`
- `updated_at`

**ProfileTranslation**

- `profile_id`
- `language` (`en` / `ja`)
- `headline`
- `summary`
- `biography`

**Skill**

- `id`
- `name`
- `category`
- `display_order`
- `created_at`
- `updated_at`

**Project**

- `id`
- `slug`
- `type` (parent category)
- `subcategory`
- `start_date`
- `end_date`
- `is_published`
- `created_at`
- `updated_at`

**ProjectTranslation**

- `project_id`
- `language` (`en` / `ja`)
- `title`
- `summary`
- `overview`
- `role`
- `architecture`
- `development_process`
- `challenges`
- `solutions`
- `results`

**ProjectSkill**

- `project_id`
- `skill_id`

**ProjectImage**

- `id`
- `project_id`
- `image_path` (Phase 1: public static-asset path)
- `alt_text`
- `caption`
- `display_order`

**ProjectLink**

- `id`
- `project_id`
- `label`
- `url`
- `link_type`
- `display_order`

### 11.4 Translation-table Rules

- Separate language-independent fields from translated text fields.
- Translation records carry a `language` value of `en` or `ja`.
- A given parent record can have at most one translation record for each language.
- Project title, summary, personal role, and applicable Markdown descriptions belong in `ProjectTranslation`.
- Profile narrative fields belong in `ProfileTranslation`.
- Untranslated official technology names remain shared fields, rather than duplicated translations.
- Static UI strings are handled by the frontend localization mechanism.
- Translation needs for remaining specific fields are resolved during basic design without changing the agreed two-language coverage.

### 11.5 Data Integrity and Publication Rules

- Every project has exactly one parent category and one corresponding subcategory.
- Only defined category/subcategory combinations are valid.
- Unpublished projects **must not** be displayed on public pages.
- Public ordering uses parent category and chronology; no manual sorting is required.
- The legacy independent, multi-select `Project Experience Type` entity must not be implemented.
- Exact SQL implementation (`CHECK` constraints, reference tables, etc.) is determined during database design.
- Supabase Row Level Security requirements must be assessed for the implemented access model; server-side authorization and publication filtering remain necessary when using privileged database connections.

## 12. Architecture, Hosting, and Delivery

### 12.1 Confirmed Services

| Responsibility | Technology / Service | Phase |
|---|---|---|
| Frontend application | Next.js + TypeScript | 1 |
| Frontend hosting | Vercel | 1 |
| REST API and business logic | Java + Spring Boot | 1 |
| API hosting | Azure Container Apps | 1 |
| Database | Supabase PostgreSQL Free | 1 |
| Email delivery | Resend | 1 |
| Static project-image delivery | Next.js `public` / Vercel | 1 |
| API CI/CD | GitHub Actions | 1 |
| Frontend deployment integration | Vercel Git integration | 1 |
| Azure subscription | Azure for Students | 1 |
| Authentication | Supabase Auth + Java-side security integration | 2 |
| Managed image storage | Supabase Storage | 2 |

The particular Java release, Spring Boot release, ORM, migration library, deployment container configuration, and detailed API contract are basic-design choices, not new requirements approved here.

### 12.2 Domain and DNS

- Use an **existing, personally owned domain** for the public site.
- Point the chosen site hostname to Vercel as part of deployment.
- Verify a controlled domain with Resend for outbound email.
- Final hostnames, DNS records, `www` policy, and sender address are deployment configuration details.

### 12.3 Hosting and Operational Constraints

- Azure for Students is the intended subscription for the Java backend.
- Vercel Hobby is a **candidate** for the employment-focused website; its applicable terms must be checked before launch. This requirements document does not guarantee eligibility.
- The architecture does not eliminate API cold starts, potential Supabase Free inactivity pauses, or ongoing infrastructure costs.
- Evaluate those risks during basic design and deployment.

### 12.4 API Access and Browser Integration

- Spring Boot provides the public-facing REST interface for application data.
- The exact API endpoints and HTTP contracts are defined in basic design.
- Whether requests originate from Next.js server-side rendering or directly from the browser is a subsequent implementation decision; configure CORS appropriately if browser-to-API cross-origin requests are used.

## 13. Phases and Release Scope

### 13.1 Phase 1 — Public Portfolio (**Approved**)

Deliver:

- The six public pages defined in Section 4
- Japanese/English UI and fully translated project descriptions
- English as default with locale-specific URLs
- Hierarchical project classification and filters
- Professional-first, newest-first project ordering
- Initial database content via Spring Boot API
- Responsive design and suitable existing interactions
- Contact email via Resend
- Honeypot and rate limiting
- Static screenshot delivery via Vercel
- Caching of public content
- Deployment to the approved Vercel/Azure/Supabase architecture

Phase 1 does **not** include the administration, editing, upload, or AI features noted in Section 4.3.

### 13.2 Phase 2 — Administration and AI (**Future Candidate**)

Potential features:

- Administrator authentication and access control
- Administration interface
- Editing profile information
- Creating, editing, deleting, and publishing projects
- Tag administration
- Image upload/management with Supabase Storage
- AI chatbot

These are roadmap candidates, **not** Phase 1 acceptance criteria.

### 13.3 Phase 3 — Further Enhancements (**Future Candidate**)

Potential features:

- Featured projects / pinning
- Manual project ordering
- Contributions and Resume pages
- Enhanced SEO and Open Graph metadata
- Analytics
- Advanced operational capabilities
- Further UI/UX improvements

## 14. Non-functional Requirements

### 14.1 Responsive Design

Support mobile, tablet, and desktop layouts.

### 14.2 Performance and Caching

- Aim for fast initial page rendering.
- Optimize image delivery.
- Avoid unnecessary animation.
- Minimize database/API requests where practicable.
- Provide appropriate loading and error states.
- Cache public profile, skills, and project content on Next.js/Vercel using **ISR or an equivalent compatible strategy**.
- Immediate publication of changed content is **not** required; cache revalidation may delay visibility of updates.
- Cache behavior should reduce API load and improve resilience to temporary downstream failures, but **does not guarantee availability** when no valid cache exists.
- Set revalidation intervals, cache keys, invalidation strategy, and error behavior in basic design.
- Do **not** cache contact-form submission operations as public page data.

### 14.3 Accessibility

Provide, at minimum:

- Keyboard-operable controls
- Appropriate heading structure
- Meaningful image alternative text
- Sufficient color contrast
- Labeled form controls
- Understandable validation and error messages
- Consideration of reduced-motion user preferences

### 14.4 Security

- Validate all contact submissions server-side.
- Apply honeypot checks and server-side rate limiting before email delivery.
- Never expose API keys, credentials, or secrets to the browser.
- Manage secrets through deployment secrets/environment configuration.
- Do not expose private or unpublished project data through public endpoints.
- Assess and configure Supabase RLS in a manner consistent with actual database access rights.
- Administration authentication/authorization is a Phase 2 requirement, not a Phase 1 feature.

### 14.5 Reliability and Cost Awareness

- Use the agreed public-data cache to improve resilience, subject to its documented limitations.
- Account for Azure cold starts and Supabase Free availability constraints.
- Monitor resource usage and avoid assuming all supporting Azure services are free.
- Specific monitoring thresholds, uptime targets, and operational runbooks are deferred to deployment design.

## 15. Items Reserved for Basic Design or Content Design

These items are **not blockers to Phase 1 requirements approval**. Their eventual decisions must remain consistent with the requirements above.

### 15.1 Basic / Detailed Design

1. REST endpoints, DTOs, validation contracts, and error responses.
2. Database physical schema, data types, foreign keys, uniqueness rules, and migration strategy.
3. ORM/SQL approach and Supabase database connection/pooling configuration.
4. Exact frontend caching/ISR strategy, intervals, invalidation, and error states.
5. Client/server request placement, CORS, and any applicable API security boundaries.
6. Rate-limiting policy, honeypot implementation, and testing strategy.
7. Sender address, domain verification, DNS, and final application hostnames.
8. Precise project date representations, URL patterns, and image/link metadata behavior.
9. Runtime/framework versions and CI/CD workflow details.

### 15.2 UI / Content Design

- Photo/avatar presence
- Number of representative projects on Home
- Skill proficiency and years-of-experience display
- Skill category layout and icons
- Screenshot count and final presentation
- Detailed content copy and any final editorial revisions
- Exact visual treatment of image galleries and project links

### 15.3 Later-Phase Decisions

- Administration permission details
- Supabase Storage migration procedure
- AI chatbot requirements
- Contributions, Resume, analytics, and advanced SEO features

## 16. Decision Register

The following records preserve the identifiers used during requirements discussions. **D-009 is superseded**; the other listed decisions remain approved for Phase 1 to the extent applicable.

| ID | Decision | Status |
|---|---|---|
| D-001 | Target roles: Java Backend Engineer / Full-Stack Engineer | Approved |
| D-002 | Phase 1 pages: Home, About, Skills, Experience, Project Detail, Contact | Approved |
| D-003 | Contributions, Resume, and AI chatbot excluded from Phase 1 | Approved |
| D-004 | Supported languages: English and Japanese | Approved |
| D-005 | Default language: English | Approved |
| D-006 | Improve the existing `Portfolio-Next.js` design | Approved |
| D-007 | Retain suitable animation, interaction, and responsive behavior | Approved |
| D-008 | Parent categories: Professional and Personal Project | Approved |
| D-009 | Previous independent, multi-select experience types | **Superseded by D-028** |
| D-010 | Professional appears before Personal Project | Approved |
| D-011 | Within each group, display newest projects first | Approved |
| D-012 | Featured-project behavior excluded from Phase 1 | Approved |
| D-013 | Prioritize project overview and personal role | Approved |
| D-014 | Generate contact email subject in the system | Approved |
| D-015 | Do not persist contact messages in the database | Approved |
| D-016 | Administration interface belongs to Phase 2 | Approved |
| D-017 | Use a database beginning in Phase 1 | Approved |
| D-018 | Database service: Supabase PostgreSQL | Approved |
| D-019 | Start with Supabase Free | Approved |
| D-020 | Deploy Next.js to Vercel and Spring Boot API to Azure Container Apps | Approved |
| D-021 | Use Azure for Students for the Azure deployment | Approved |
| D-022 | Separate locale URLs: `/en/...` and `/ja/...` | Approved |
| D-023 | Do not persist language preference; `/` redirects to `/en` | Approved |
| D-024 | Maintain complete project-detail descriptions in both languages | Approved |
| D-025 | Preserve official technology and proper names | Approved |
| D-026 | Store long-form project descriptions as Markdown in separate fields | Approved |
| D-027 | Require title, summary, personal role, category, start period, slug, publication state; optional fields may be omitted | Approved |
| D-028 | Require exactly one parent category and one valid subcategory; supersede D-009 | Approved |
| D-029 | Single-select parent filter; initial value `All Projects` | Approved |
| D-030 | Single-select subcategory filter; initial value `All` | Approved |
| D-031 | Reset subcategory to `All` whenever parent category changes | Approved |
| D-032 | `Employment` is the paid-employment subcategory | Approved |
| D-033 | `Personal Learning Project` is the self-directed-learning subcategory | Approved |
| D-034 | Add `Independent Project` to Personal Project; no additional special-case behavior now | Approved |
| D-035 | Runtime database access through Spring Boot REST API; future Supabase Auth integration excepted | Approved |
| D-036 | Vercel + Azure Container Apps + Supabase; GitHub Actions and Vercel Git integration; Azure for Students | Approved |
| D-037 | Send contact emails via Resend from the Spring Boot API; secure the API key on Azure | Approved |
| D-038 | Spam protection: honeypot plus server-side rate limiting; no Turnstile in Phase 1 | Approved |
| D-039 | Use translation tables with `language` of `en` or `ja`; finalize physical schema later | Approved |
| D-040 | Phase 1 images in Next.js `public`, managed in Git; Storage migration later | Approved |
| D-041 | Cache public data in Next.js/Vercel using ISR or equivalent; finalize intervals later | Approved |
| D-042 | Use an existing owned domain for the site and Resend verification; finalize DNS later | Approved |
| D-043 | Employment-focused site; no active freelance lead generation in Phase 1; check Vercel plan terms | Approved |

## 17. Phase 1 Acceptance Checklist

The checks below are derived from the approved requirements, and are intended to support implementation review and testing. They do not introduce additional product scope.

- [ ] `/` redirects to `/en`; English and Japanese URLs render their respective language content.
- [ ] Language switching works without saving a language-preference cookie/local-storage entry.
- [ ] All six public pages are available on supported screen sizes.
- [ ] Profile, skills, and published projects load through the Spring Boot API.
- [ ] The parent/subcategory project classification and single-select filters behave as specified.
- [ ] Changing parent category resets the subcategory filter to `All`.
- [ ] Project ordering is Professional first, with newest entries first within each group.
- [ ] Unpublished projects are not exposed by public listings or detail endpoints.
- [ ] Project details support English and Japanese content and formatted Markdown.
- [ ] Optional project-detail sections are hidden when absent; available static images load from Vercel.
- [ ] Contact form input is validated and protected by server-side honeypot and rate limiting.
- [ ] Successful contact submissions invoke Resend through Spring Boot without database storage of the message.
- [ ] No secrets or privileged database credentials are exposed to browsers or committed to Git.
- [ ] Public content uses the approved caching strategy, with documented behavior during downstream failures.
- [ ] API and frontend are deployed to the approved platforms with automated delivery workflows.
- [ ] Keyboard interaction, accessible labels, alternative text, and reduced-motion considerations are addressed.

## 18. AI-assisted Development Principles

The project will use an AI coding agent while retaining human understanding, design approval, and code review. Development starts **only after Phase 1 requirements approval**, and follows incremental implementation and testable milestones.

### 18.1 Development Guidelines

1. Implement **one feature at a time**.
2. Follow this approved requirements document; do not expand scope without approval.
3. Explain design decisions before implementing complex features.
4. Prefer simple, maintainable solutions.
5. Apply SOLID principles when useful without unnecessary abstractions.
6. Include appropriate automated tests for implemented features.
7. Explain unfamiliar technologies and concepts to the project owner.
8. Do not add dependencies without explaining why.
9. Do not modify unrelated files.
10. Keep pull requests small enough for focused review.
11. Never commit credentials, secrets, or local environment files.
12. After each change, summarize what changed, why it works, and how to test it.
13. Obtain human review and approval before proceeding to the next significant feature.

### 18.2 Recommended Implementation Roadmap

1. **Basic design:** confirm architecture, API contracts, database ER model, and technology versions.
2. **Minimal Java API:** implement and test a small Spring Boot REST endpoint with hard-coded data.
3. **Database integration:** set up Supabase PostgreSQL, migrations, and data access through Spring Boot.
4. **Frontend integration:** reuse/improve `Portfolio-Next.js` and connect it to the API; deploy on Vercel.
5. **Azure deployment:** containerize Spring Boot and deploy on Azure Container Apps; configure secrets and networking.
6. **Complete Phase 1:** implement all pages, localization, classification/filtering, contact flow, caching, security basics, and tests.
7. **Phase 2 later:** only after Phase 1 verification, consider Supabase Auth, administration, Supabase Storage, and AI features.

### 18.3 Working Agreement

- The **requirements document** defines *what* must be built.
- The **basic-design artifacts** define *how* the approved behavior is implemented.
- The AI agent proposes and implements changes; the project owner reviews their effect and learns the underlying technology.
- New scope is discussed and explicitly approved before being added.

---

**Approval record:** The project owner approved finalization of the Phase 1 requirements on November 24, 2026. This English edition consolidates the previously agreed decisions (D-001 through D-043), removes superseded draft questions, and reserves implementation-specific detail for basic design.