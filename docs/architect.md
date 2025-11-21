# Architecture — Smart Workforce & Task Management (one-page)

## Goal
Provide a secure, scalable solution for creating, assigning, tracking, and auditing tasks using Microsoft Power Platform and Azure. Demonstrates PL-400 skills across design, extension, integration, ALM, security, and monitoring.

## Users & Roles
- **Manager**: use Model-driven app to manage projects and approve tasks
- **Employee**: use Canvas mobile app to accept tasks and update progress
- **Admin/DevOps**: manage environments, security, and CI/CD

## Core Components
1. **Power Apps**
   - Model-driven app (manager portal)
   - Canvas app (employee mobile)
   - PCF controls embedded in forms and dashboards
2. **Dataverse**
   - Primary data store (tables listed in datamodel.md)
   - Security roles and row-level sharing
   - Change tracking enabled on sync tables
3. **Server-side code**
   - Dataverse plug-ins (validation, assignment, audit)
   - Dataverse Custom API (GenerateProjectSummary)
4. **Integrations & Compute (Azure)**
   - Azure Functions for heavy compute and scheduled tasks
   - Azure Service Bus / Event Hub for events
   - Custom connector to call Azure Functions
   - Azure Key Vault for secrets
5. **Automation**
   - Power Automate cloud flows for approvals, notifications, error handling
6. **ALM & DevOps**
   - Solutions in source control
   - Power Platform CLI (`pac`) and Power Platform Build Tools for pipelines
   - GitHub Actions or Azure DevOps pipelines for CI/CD
7. **Auth & Security**
   - Microsoft Entra ID (Azure AD) for user auth
   - Managed Identity for Azure Functions to call Dataverse
   - Service principal (app registration) for automated pipelines
   - Principle of least privilege for Dataverse security roles
8. **Monitoring & Debug**
   - App Insights for Azure Functions
   - Plugin Trace Logs & Profiler
   - Monitor tool for Canvas apps

## Where business logic lives (summary)
- **Client(UI)**: Power Fx + Canvas components + JS client scripts — for UI validation, navigation, lightweight logic.
- **Server (Dataverse)**: Plug-ins & Custom API — for transactional validation, concurrency-safe business logic.
- **Cloud**: Azure Functions — heavy compute, long-running tasks, external integrations.
- **Flows**: Orchestration, approvals, notifications, child flows, error handling.

## Dataflow (simple)
1. Manager creates Task (Model-driven) → Dataverse record created → synchronous plug-in sets owner, pre/post images used for validation.
2. Power Automate approval flow triggers on Task create → if approved → call Azure Function via custom connector to compute assignment schedule → emit event to Service Bus.
3. Azure Function processes and returns schedule → updates Dataverse via Web API using managed identity.
4. Employee receives notification in Canvas app and updates Task → Flow updates project KPIs and writes to AuditLog (elastic table).

## Non-functional concerns
- Performance: Query delegation in canvas, indexing & alternate keys in Dataverse, bulk operation design.
- Security: Row-level security, DLP policies, encrypt secrets in Key Vault.
- Scalability: Event-driven architecture using Service Bus and Azure Functions.
- Observability: Logs, telemetry, Monitor traces.

## Files to check
- `docs/datamodel.md` — ERD and table details
- `src/plugins` — plug-in code
- `connectors/*` — OpenAPI specs
- `src/azure-functions` — functions code
- `ci-cd/*` — pipeline configs
