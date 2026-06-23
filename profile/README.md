<div align="center">

<img src="assets/eron_logo_small.svg" alt="Eron World" width="104" />

# Eron World

**Digitaliza tu obra** — a B2B construction-management platform for Spanish-speaking teams.

[![Status](https://img.shields.io/badge/status-active_development-4ba792?style=flat-square)](https://github.com/Eron-World)
[![Stage](https://img.shields.io/badge/stage-alpha-4ba792?style=flat-square)](https://github.com/Eron-World)
[![Cloud](https://img.shields.io/badge/cloud-Microsoft_Azure-0078D4?style=flat-square)](https://azure.microsoft.com)
![Product](https://img.shields.io/badge/product_language-es--ES-555?style=flat-square)
[![License](https://img.shields.io/badge/license-proprietary-555?style=flat-square)](#license)

</div>

---

## What we build

Eron World is a construction-management SaaS for site teams — reporting, quality observations, and BIM data, in one authenticated workspace.

The platform began on **Microsoft Power Platform** (Power Pages portal + PowerApps Canvas apps + Dataverse) and is being migrated to a **native, cloud-native stack on Azure**: a SvelteKit web client and a C#/.NET backend, with a dedicated service that syncs Trimble Connect BIM data into the platform. Power Platform surfaces are rebuilt natively one by one and decommissioned as they reach parity.

---

## Repositories

| Repository | What it does | Stack | Status |
| :--------- | :----------- | :---- | :----- |
| [**eron-client-sv**](https://github.com/Eron-World/eron-client-sv) | User-facing web client — auth gate, app launcher, and native data surfaces (Reportes, Observaciones de Calidad, Trimble Sync). | SvelteKit · Svelte 5 · TypeScript | ![](https://img.shields.io/badge/-deployed-4ba792?style=flat-square) `v0.4.3-alpha` |
| [**eron-backend-csharp**](https://github.com/Eron-World/eron-backend-csharp) | REST API backend — domain logic and the data layer migrating off Dataverse to Azure SQL. | .NET 10 · C# · ASP.NET Core | ![](https://img.shields.io/badge/-in_development-f59e0b?style=flat-square) `0.1.0-alpha` |
| [**eron-trimble-connect-adapter**](https://github.com/Eron-World/eron-trimble-connect-adapter) | Bidirectional sync between Trimble Connect BIM data and Dataverse, with an admin portal. | Azure Functions · .NET 10 · React | ![](https://img.shields.io/badge/-in_development-f59e0b?style=flat-square) |

> Repositories are **private**; the links resolve for members. Access is granted by role.

### eron-client-sv — web client

[![SvelteKit](https://img.shields.io/badge/SvelteKit-FF3E00?style=flat-square&logo=svelte&logoColor=white)](https://kit.svelte.dev)
[![Svelte 5](https://img.shields.io/badge/Svelte_5_(runes)-FF3E00?style=flat-square&logo=svelte&logoColor=white)](https://svelte.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![SCSS](https://img.shields.io/badge/SCSS-CC6699?style=flat-square&logo=sass&logoColor=white)](https://sass-lang.com)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev)
[![pnpm](https://img.shields.io/badge/pnpm-F69220?style=flat-square&logo=pnpm&logoColor=white)](https://pnpm.io)
[![Azure SWA](https://img.shields.io/badge/Azure_SWA-0078D4?style=flat-square)](https://azure.microsoft.com/products/app-service/static)
[![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square)](https://playwright.dev)

Authenticated client on Azure Static Web Apps. Sign-in via Microsoft Entra (OIDC/MSAL), app catalog from Dataverse, transactional email through Microsoft Graph. Native surfaces: **Reportes V2** (read-only tables + charts), **Observaciones de Calidad V2** (closure workflow with row-level security), and the **Trimble Sync** dashboard.

### eron-backend-csharp — REST API

[![.NET](https://img.shields.io/badge/.NET_10-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com)
[![C#](https://img.shields.io/badge/C%23-512BD4?style=flat-square)](https://learn.microsoft.com/dotnet/csharp)
[![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/apps/aspnet)
[![EF Core](https://img.shields.io/badge/EF_Core-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://learn.microsoft.com/ef/core)
[![Azure SQL](https://img.shields.io/badge/Azure_SQL-CC2927?style=flat-square)](https://azure.microsoft.com/products/azure-sql)
[![Container Apps](https://img.shields.io/badge/Container_Apps-0078D4?style=flat-square)](https://azure.microsoft.com/products/container-apps)
[![Bicep](https://img.shields.io/badge/Bicep-0078D4?style=flat-square)](https://learn.microsoft.com/azure/azure-resource-manager/bicep)

Clean-architecture solution (`Domain` ▸ `Application` ▸ `Api`) with swappable infrastructure adapters (`Azure` · `Dataverse` · `SqlServer` · `Trimble`). Today it adapts Dataverse; persistence is migrating to Azure SQL behind stable ports. Infrastructure is provisioned as code (Bicep) across development, preview, and production.

### eron-trimble-connect-adapter — BIM sync

[![.NET](https://img.shields.io/badge/.NET_10-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com)
[![Azure Functions](https://img.shields.io/badge/Azure_Functions-0062AD?style=flat-square)](https://azure.microsoft.com/products/functions)
[![React](https://img.shields.io/badge/React_18-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev)
[![Trimble Connect](https://img.shields.io/badge/Trimble_Connect-0063A3?style=flat-square&logo=trimble&logoColor=white)](https://connect.trimble.com)

Azure Functions v4 backend with a React/Vite admin portal. Keeps BIM objects from Trimble Connect in sync with Dataverse via transparent PKCE auth, so model data is available to the rest of the platform.

---

## Architecture

```mermaid
flowchart LR
    users(["Site teams"])

    subgraph client["eron-client-sv"]
        web["Web client<br/>Azure Static Web Apps"]
    end

    subgraph backend["eron-backend-csharp"]
        api["REST API<br/>ASP.NET Core"]
    end

    subgraph sync["eron-trimble-connect-adapter"]
        fn["Azure Functions"]
    end

    entra{{"Microsoft Entra · OIDC / MSAL"}}
    dv[("Dataverse")]
    sql[("Azure SQL")]
    tc["Trimble Connect"]

    users -->|HTTPS| web
    web -->|API| api
    api --> dv & sql
    fn <-->|sync| tc
    fn --> dv
    entra -. authenticates .-> web
    entra -. authenticates .-> api
```

---

## Roadmap

Tracked on the private [project board](https://github.com/orgs/Eron-World/projects/2). Highlights:

**Shipped**
- Authentication gate (Microsoft Entra / MSAL) with invite flow
- App launcher embedding the legacy PowerApps Canvas apps
- **Reportes V2** — native rebuild with charts and PDF/Excel export
- **Observaciones de Calidad V2** — native closure workflow with row-level security
- **Trimble Sync** — read-only sync dashboard
- Custom domains, production + preview deployments, SEO/indexing
- C#/.NET backend foundation (clean architecture, backend-driven OAuth)

**In progress**
- Data-layer migration: Dataverse → Azure SQL behind stable ports
- Backend-driven authentication and a clean RFC 9457 `problem+json` API contract

**Next**
- Decommission the legacy Power Pages portal once native parity is reached
- Validate the native rebuilds against the legacy Canvas apps (performance, UX, velocity)
- Trimble Sync write surface (property CRUD)

---

## Engineering standards

- **Gated CI** — no change reaches production without passing the quality gate (type-check, lint, unit, end-to-end), followed by post-deploy smoke checks.
- **Change control** — promotion is sequential and gated; direct pushes to production branches are not allowed.
- **Automated review** — every change goes through automated code review and dependency monitoring.
- **Privacy** — no personal data in URLs or telemetry; users are identified by opaque directory IDs.
- **Language** — all user-facing copy is **es-ES** (Spain Spanish).

---

## License

Proprietary. © Eron World. All rights reserved unless a repository states otherwise.

## Maintainer

**Gabriel Barnada** — [@glovek08](https://github.com/glovek08) · [eron.world](https://eron.world)
