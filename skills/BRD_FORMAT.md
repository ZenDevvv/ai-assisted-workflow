# BRD Format Standard

## Purpose

This skill defines the exact structure and conventions for the Business Requirements Document (BRD). The BRD is the persistent anchor for the entire development workflow — every downstream phase references it. This format ensures:

1. **Human reviewability** — Clean Markdown that reads naturally during manual review.
2. **Machine parseability** — Strict heading conventions and requirement IDs that allow the orchestrator to programmatically slice the BRD by module, extract acceptance criteria, and pull error states for downstream phases.
3. **Traceability** — Every functional requirement has a unique ID that can be referenced in architecture, tests, and code review.

## Rules

### Document Structure

The BRD **must** follow this exact top-level heading structure in this order:

```
# [Project Name] — Business Requirements Document
## 1. Overview
## 2. Objectives
## 3. User Roles
## 4. Modules
### 4.x [MODULE_ID] — [Module Name]
#### 4.x.1 Requirements
##### [REQ_ID] — [Requirement Title]
#### 4.x.2 Error States
## 5. Non-Functional Requirements
## 6. Assumptions & Constraints
## 7. Out of Scope
```

### Heading Conventions

- **H1** (`#`): Document title only. Exactly one per document.
- **H2** (`##`): Top-level sections. Numbered sequentially (`## 1.`, `## 2.`, etc.).
- **H3** (`###`): Module headers inside `## 4. Modules`. Numbered as `### 4.1`, `### 4.2`, etc.
- **H4** (`####`): Subsections within a module (`Requirements`, `Error States`).
- **H5** (`#####`): Individual requirements within a module.

### Module IDs

Every module **must** have a short, uppercase, snake_case identifier in its heading:

```markdown
### 4.1 AUTH — Authentication & Authorization
### 4.2 USERS — User Management
### 4.3 PROJECTS — Project Management
### 4.4 NOTIFICATIONS — Notification System
```

The `MODULE_ID` (e.g., `AUTH`, `USERS`, `PROJECTS`) is used by the orchestrator to:
- Slice the BRD for module-level phases (4, 5, 9, 10, 11)
- Name output directories (`outputs/phase-04/auth/`, `outputs/phase-04/users/`)
- Match modules across phases

**Rules for MODULE_ID:**
- Uppercase, snake_case (e.g., `USER_PROFILES`, `AUDIT_LOG`)
- Unique across the entire BRD
- Short but descriptive (1–3 words)
- Stable — once assigned, do not rename across phases

### Requirement IDs

Every functional requirement **must** have a unique ID in the format:

```
[MODULE_ID]-[NNN]
```

Examples: `AUTH-001`, `AUTH-002`, `USERS-001`, `PROJECTS-001`

**Rules for Requirement IDs:**
- Sequential within each module, starting at `001`
- Never reuse an ID, even if a requirement is removed (skip the number)
- The ID appears in the H5 heading: `##### AUTH-001 — User Registration`

### Requirement Structure

Every requirement under `#### x.x.1 Requirements` **must** include these subsections in this order:

```markdown
##### [REQ_ID] — [Requirement Title]

**Description:**
[1–3 sentences describing what this requirement does from the user's perspective.]

**Acceptance Criteria:**
- GIVEN [precondition], WHEN [action], THEN [expected outcome]
- GIVEN [precondition], WHEN [action], THEN [expected outcome]

**Error States:**
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`

**Priority:** [Must-have | Should-have | Nice-to-have]

**Notes:**
[Optional. Any additional context, constraints, or edge cases not captured above.]
```

### Acceptance Criteria Format

Acceptance criteria **must** use the Given/When/Then format:

```
- GIVEN [precondition], WHEN [action], THEN [expected outcome]
```

**Rules:**
- Each criterion is a single bullet point (no nested bullets)
- Start with `GIVEN`, `WHEN`, `THEN` in uppercase
- Each criterion must be independently testable
- Avoid vague outcomes — use specific, observable behaviors
- Minimum 2 acceptance criteria per requirement

**Good example:**
```
- GIVEN a registered user, WHEN they submit valid credentials, THEN they receive an access token and refresh token
- GIVEN a registered user, WHEN they submit an incorrect password 5 times, THEN the account is locked for 15 minutes
```

**Bad example:**
```
- The login should work properly
- Users can log in with their credentials
```

### Error States Format

Error states **must** follow this format:

```
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`
```

**Rules:**
- Each error state is a single bullet point
- The `ERROR_CODE` is a short, uppercase identifier (e.g., `INVALID_CREDENTIALS`, `RATE_LIMITED`, `NOT_FOUND`)
- Error codes should be descriptive and unique within the module
- Include the user-facing behavior, not just the technical response

**Good example:**
```
- WHEN email format is invalid, THEN show inline validation error "Please enter a valid email" → `INVALID_EMAIL`
- WHEN account is locked, THEN show error "Account temporarily locked. Try again in {minutes} minutes" → `ACCOUNT_LOCKED`
```

### Module-Level Error States Section

In addition to per-requirement error states, each module has a `#### x.x.2 Error States` section that lists **cross-cutting** error states that apply to the module as a whole (e.g., authorization failures, rate limits):

```markdown
#### 4.1.2 Error States

| Error Code          | Condition                          | User-Facing Behavior                              |
|---------------------|------------------------------------|----------------------------------------------------|
| `UNAUTHORIZED`      | Missing or expired token           | Redirect to login page                             |
| `FORBIDDEN`         | Valid token but insufficient role   | Show "You don't have permission" message           |
| `RATE_LIMITED`      | Too many requests in time window   | Show "Too many attempts. Try again in {seconds}s"  |
```

### Section Details

#### `## 1. Overview`
- 2–4 sentences describing what the application is and who it serves.
- No technical implementation details.

#### `## 2. Objectives`
- Bulleted list of 3–7 measurable project objectives.
- Each objective should be verifiable (not vague like "improve UX").

#### `## 3. User Roles`
- Table format listing each role and what it can do:

```markdown
| Role    | Description                        | Key Permissions                          |
|---------|------------------------------------|------------------------------------------|
| Admin   | Full system access                 | Manage users, configure settings, ...    |
| Member  | Standard authenticated user        | Create, read, update own resources, ...  |
| Guest   | Unauthenticated visitor            | View public content only                 |
```

#### `## 4. Modules`
- This is the core of the BRD. All functional requirements live here.
- Each module is a self-contained group of related features.
- Module ordering should follow the independent-first principle (modules with no dependencies on other modules come first).

#### `## 5. Non-Functional Requirements`
- Performance, security, scalability, accessibility requirements.
- Use measurable targets where possible (e.g., "API response < 200ms at p95").
- Not grouped by module — these apply globally.

#### `## 6. Assumptions & Constraints`
- Technical assumptions (e.g., "PostgreSQL as primary database").
- Business constraints (e.g., "Must launch by Q3 2025").
- Integration assumptions (e.g., "Email service via SendGrid API").

#### `## 7. Out of Scope`
- Explicit list of features or capabilities **not** included in this project.
- Prevents scope creep and sets expectations.


## Templates

### Full BRD Template

```markdown
# [Project Name] — Business Requirements Document

## 1. Overview

[2–4 sentences. What is this application? Who is it for? What problem does it solve?]

## 2. Objectives

- [Measurable objective 1]
- [Measurable objective 2]
- [Measurable objective 3]

## 3. User Roles

| Role        | Description                    | Key Permissions                              |
|-------------|--------------------------------|----------------------------------------------|
| [Role 1]    | [What this role represents]    | [Comma-separated list of key permissions]    |
| [Role 2]    | [What this role represents]    | [Comma-separated list of key permissions]    |

## 4. Modules

### 4.1 [MODULE_ID] — [Module Name]

#### 4.1.1 Requirements

##### [MODULE_ID]-001 — [Requirement Title]

**Description:**
[1–3 sentences from the user's perspective.]

**Acceptance Criteria:**
- GIVEN [precondition], WHEN [action], THEN [expected outcome]
- GIVEN [precondition], WHEN [action], THEN [expected outcome]

**Error States:**
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`

**Priority:** [Must-have | Should-have | Nice-to-have]

**Notes:**
[Optional context.]

##### [MODULE_ID]-002 — [Requirement Title]

**Description:**
[...]

**Acceptance Criteria:**
- GIVEN [...], WHEN [...], THEN [...]

**Error States:**
- WHEN [...], THEN [...] → `[ERROR_CODE]`

**Priority:** [...]

#### 4.1.2 Error States

| Error Code          | Condition                          | User-Facing Behavior                    |
|---------------------|------------------------------------|-----------------------------------------|
| `[ERROR_CODE]`      | [When this happens]                | [What the user sees]                    |

---

### 4.2 [MODULE_ID] — [Module Name]

[Repeat structure...]

## 5. Non-Functional Requirements

- **Performance:** [e.g., API response < 200ms at p95]
- **Security:** [e.g., All endpoints require authentication except public routes]
- **Scalability:** [e.g., Support up to 10,000 concurrent users]
- **Accessibility:** [e.g., WCAG 2.1 AA compliance]

## 6. Assumptions & Constraints

- [Assumption or constraint 1]
- [Assumption or constraint 2]

## 7. Out of Scope

- [Feature or capability explicitly excluded]
- [Feature or capability explicitly excluded]
```

### Single Requirement Template

```markdown
##### [MODULE_ID]-[NNN] — [Requirement Title]

**Description:**
[1–3 sentences from the user's perspective.]

**Acceptance Criteria:**
- GIVEN [precondition], WHEN [action], THEN [expected outcome]
- GIVEN [precondition], WHEN [action], THEN [expected outcome]

**Error States:**
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`
- WHEN [error condition], THEN [system behavior] → `[ERROR_CODE]`

**Priority:** [Must-have | Should-have | Nice-to-have]

**Notes:**
[Optional.]
```


## Examples

### Good Module Example

```markdown
### 4.1 AUTH — Authentication & Authorization

#### 4.1.1 Requirements

##### AUTH-001 — User Registration

**Description:**
A new user can create an account by providing their email, password, and display name. The system sends a verification email before the account becomes active.

**Acceptance Criteria:**
- GIVEN a visitor, WHEN they submit a valid email, password (min 8 chars, 1 uppercase, 1 number), and display name, THEN an account is created with "pending" status and a verification email is sent
- GIVEN a visitor, WHEN they click the verification link within 24 hours, THEN the account status changes to "active" and they are redirected to the login page
- GIVEN a visitor, WHEN they click an expired verification link, THEN they see an error message with an option to resend the verification email

**Error States:**
- WHEN email is already registered, THEN show "An account with this email already exists" → `EMAIL_EXISTS`
- WHEN password does not meet complexity requirements, THEN show inline validation with specific missing criteria → `WEAK_PASSWORD`
- WHEN display name exceeds 50 characters, THEN show "Display name must be 50 characters or less" → `NAME_TOO_LONG`

**Priority:** Must-have

##### AUTH-002 — User Login

**Description:**
A registered user can log in with their email and password to receive an access token and refresh token.

**Acceptance Criteria:**
- GIVEN a registered user with an active account, WHEN they submit valid credentials, THEN they receive an access token (15min expiry) and refresh token (7day expiry)
- GIVEN a registered user, WHEN they submit an incorrect password, THEN they see "Invalid email or password" (no indication of which is wrong)
- GIVEN a registered user, WHEN they fail login 5 times within 10 minutes, THEN the account is locked for 15 minutes

**Error States:**
- WHEN credentials are invalid, THEN show "Invalid email or password" → `INVALID_CREDENTIALS`
- WHEN account is locked, THEN show "Account temporarily locked. Try again in {minutes} minutes" → `ACCOUNT_LOCKED`
- WHEN account is not verified, THEN show "Please verify your email first" with resend option → `UNVERIFIED_ACCOUNT`

**Priority:** Must-have

#### 4.1.2 Error States

| Error Code             | Condition                                   | User-Facing Behavior                                          |
|------------------------|---------------------------------------------|---------------------------------------------------------------|
| `UNAUTHORIZED`         | Missing or expired access token             | Redirect to login page                                        |
| `FORBIDDEN`            | Valid token but insufficient role            | Show "You don't have permission to access this resource"      |
| `TOKEN_EXPIRED`        | Refresh token has expired                   | Redirect to login page with "Session expired" message         |
| `RATE_LIMITED`         | Too many auth requests in time window       | Show "Too many attempts. Please try again later"              |
```

### Bad Example — What to Avoid

```markdown
### Authentication

- Users should be able to log in
- There should be registration
- Password reset would be nice
- Use JWT tokens
- Handle errors appropriately
```

**Why this is bad:**
- No module ID — orchestrator can't slice it
- No requirement IDs — can't trace to tests or architecture
- No acceptance criteria — can't generate meaningful tests
- No error states — downstream phases have to guess
- Includes implementation details ("JWT tokens") — that's Phase 3's job
- Vague requirements — "handle errors appropriately" means nothing


## Anti-Patterns

1. **Implementation in requirements** — The BRD describes *what*, not *how*. Never mention specific technologies, database schemas, API shapes, or code patterns. Those belong in Phase 3+.

2. **Vague acceptance criteria** — "Should work correctly" or "Must be user-friendly" are not testable. Every criterion must describe a specific precondition, action, and observable outcome.

3. **Missing error states** — If a requirement can fail, it must document how it fails. A requirement with zero error states is almost always incomplete.

4. **Cross-module requirements in a single module** — If a requirement spans multiple modules (e.g., "When a user deletes their account, all their projects are also deleted"), it belongs in the module that initiates the action, with a cross-reference note:

   ```
   **Notes:**
   Cross-module impact: Triggers cascade delete in PROJECTS module (see PROJECTS-005).
   ```

5. **Unstable module IDs** — Once a MODULE_ID is assigned (e.g., `AUTH`), it must never change. All downstream phases reference this ID.

6. **Skipping the error states table** — The module-level error states table (section x.x.2) captures cross-cutting errors that don't belong to any single requirement. Skipping this leaves gaps in Phase 3's error standards.

7. **Priority without justification** — Every requirement has a priority. If everything is "Must-have", the prioritization is meaningless. Aim for roughly 60% Must-have, 25% Should-have, 15% Nice-to-have.
