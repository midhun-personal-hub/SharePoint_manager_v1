# SharePoint Management Platform Clone & Import

Import and deploy the complete `sharepoint_project_v01-main` repository into the AI Studio workspace exactly as designed, with zero alterations to its visual styling, core architecture, or feature set.

### User Review & Critical Decisions

> [!IMPORTANT]
> The user confirmed the following constraints:
> - **Execution Mode**: Run as-is matching the repository without modifications or artificial additions.
> - **Landing View**: Preserve the repository's default landing view (`/departments` landing directory & `/lists` navigation).
> - **Theme & Styling**: Keep the original visual theme and UI components intact.

- **Confirmed Decision 1**: Import all source files from `sharepoint_project_v01-main` directly into the project root.
- **Confirmed Decision 2**: Install exact required dependencies (`@azure/msal-browser`, `@azure/msal-react`, `@microsoft/microsoft-graph-client`, `@tanstack/react-query`, `@tanstack/react-table`, `zustand`, `zod`, `react-router-dom`, etc.) using npm.
- **Confirmed Decision 3**: Retain the existing demo data store (`demoDataStore.ts`) and Azure MSAL authentication fallback so the app works out-of-the-box in the preview environment while supporting real Entra ID credentials.

---

### 1. Overview & Core Concept

- **What It Does**: A single-page enterprise web platform for inspecting, creating, and managing SharePoint Online lists, list items, column schemas, views, and permissions via Microsoft Graph API and MSAL.
- **Target Audience / Persona**: SharePoint administrators, M365 developers, operations teams, and departmental power users managing tabular business data.
- **Key Value**: Low-latency, responsive alternative to the SharePoint web UI with instant field filtering, inline editing, column schema configuration, and built-in interactive demo fallback.

---

### 2. User Experience & Visual Design

- **Key User Flows**:
  1. **Landing & Department Directory**: User lands on the department directory showing configured SharePoint workspaces and connection status.
  2. **Lists Explorer**: Browse site lists, inspect list metadata, row counts, and site collections.
  3. **Adaptive Table View**: Dynamic data grid displaying items with type-specific renderers (text, choice tags, user avatars, dates, currencies), sorting, and search.
  4. **Record Management**: Create, edit, and delete rows with dedicated modal dialogs and form validation.
  5. **Schema & Permissions Panel**: Inspect list columns, add/edit column definitions, and review site/list permission roles.
  6. **Connection Settings**: Configure Azure Tenant ID, Client ID, Site ID, or toggle between Live Graph and Demo modes.
- **Visual Identity & Theme**:
  - *Aesthetic Direction*: Enterprise Microsoft 365 design language with Fluent-style cards, clean borders, and crisp typography.
  - *Color Palette*: Corporate navy/slate (`#0f172a`, `#1e293b`), SharePoint teal/blue accents (`#0078d4`, `#0284c7`), and semantic status indicators.
  - *Light & Dark Posture*: Full dark/light mode toggle with theme persistence via `themeStore.ts`.
- **Interactive Feedback & Motion**:
  - Modal transitions with `motion`
  - Subtle hover micro-interactions on table rows and action menus
  - Toast alerts and confirmation dialogs for destructive operations

---

### 3. Key Product Decisions & Trade-Offs

- **Decision 1: Exact Preservation of Repository Source**
  - *Chosen Approach*: Copy all code files from `sharepoint_project_v01-main` directly to the project root without redesigning or omitting modules.
  - *Why*: Satisfies user's explicit instruction ("as such as the repo dont change or add anything").
  - *Alternatives Considered*: Re-architecting into a single file or restructuring components (rejected as it violates user directive).
- **Decision 2: Dual Mode (MSAL + In-Memory Demo Data Store)**
  - *Chosen Approach*: Keep the existing MSAL auth setup and `demoDataStore.ts` intact.
  - *Why*: Allows immediate testing and interaction inside AI Studio preview without requiring users to configure Entra ID app registrations upfront, while allowing full live M365 integration when configured.

---

### 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SHAREPOINT MANAGEMENT PLATFORM                       │
└────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                      Authentication Layer (MSAL)                       │
│       • PublicClientApplication (Entra ID / Azure AD PKCE Flow)        │
│       • useAuth() Hook with Silent Token Acquisition & Demo Fallback   │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                        Navigation & App Router                         │
│       • SharePointSidebar & ThemeToggle                                │
│       • Routes: /departments, /lists, /lists/:listId, /schema          │
└───────────┬───────────────────────┬───────────────────────┬────────────┘
            │                       │                       │
            ▼                       ▼                       ▼
┌───────────────────────┐ ┌───────────────────┐ ┌────────────────────────┐
│    Lists & Records    │ │  Adaptive Table   │ │    Schema & Perms      │
│ • ListBrowser         │ │ • TanStack Table  │ │ • ColumnEditor         │
│ • ListDetail          │ │ • FieldRenderers  │ │ • ViewEditor           │
│ • ItemGrid & Form     │ │ • CRUD Modals     │ │ • PermissionsPanel     │
└───────────┬───────────┘ └─────────┬─────────┘ └───────────┬────────────┘
            │                       │                       │
            └───────────────────────┼───────────────────────┘
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                       State & Data Providers                           │
│  • Zustand Stores: listsStore, uiStore, themeStore, demoDataStore      │
│  • TanStack Query: Caching, background refetching, mutations           │
│  • Graph Client / Mock Provider: Microsoft Graph API + Demo Data       │
└────────────────────────────────────────────────────────────────────────┘
```

- **Dependencies to Install**:
  - `@azure/msal-browser`, `@azure/msal-react`
  - `@microsoft/microsoft-graph-client`
  - `@tanstack/react-query`, `@tanstack/react-table`
  - `react-router-dom`
  - `zod`, `zustand`
- **Verification Steps**:
  1. Copy all project files into root workspace.
  2. Install required packages via `install_applet_package`.
  3. Ensure `index.html` and `metadata.json` match repo.
  4. Compile applet with `compile_applet` and verify zero errors.
