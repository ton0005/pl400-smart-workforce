# pl400-smart-workforce

## Quickstart (developer)
1. Clone repo  
2. Install tools: VS Code, .NET SDK, Node.js, Power Platform CLI (`pac`), Power Platform VS Code extension, Git.  
3. Authenticate to Power Platform using `pac` (see docs/cli-auth.md or run `pac auth create`).  
4. Create a Dev environment (Power Platform Admin Center) and set it as active.  
5. Import solution or push local solution using Power Platform CLI (see `ci-cd/`).

## What to build (high-level)
- Dataverse tables: Employee, Task, Project, TaskUpdate, Notification, AuditLog, ExternalSystem (virtual).
- Model-driven app for managers.
- Canvas mobile app for employees.
- Dataverse plug-ins and Custom API.
- PCF component(s): calendar card or QR scanner.
- Azure Functions and Custom Connectors.
- Power Automate flows for approvals/notifications.
- ALM pipeline for CI/CD (GitHub Actions or Azure DevOps).
- AI Copilot integration and AI Builder model for sentiment.

## How this maps to PL-400
For each PL-400 objective, see `docs/pl400-mapping.md`. Example:
- **Extend the platform** → `src/plugins/TaskAssignPlugin.cs` (Dataverse plug-in with pre/post images).
- **Create custom connector** → `connectors/azurefunction-openapi.json`.
- **PCF control** → `pcf-controls/calendar-card`.

## Deliverables
- Solution export (.zip) in `/release`
- Source code for plug-ins, PCF, Azure Functions
- Docs: architecture, data model, pl400-mapping
- Demo video & screenshots

## Contributing
Open an issue or submit a PR. For development workflows see `ci-cd/README.md`.

## License
GNU
