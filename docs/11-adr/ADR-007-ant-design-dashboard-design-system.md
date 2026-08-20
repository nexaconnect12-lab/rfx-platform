# ADR-007: Ant Design Dashboard Design System

- Status: Accepted
- Decision date: 2026-08-20
- Accepted by: Project owner

## Context

The RFx web dashboard needs a consistent design system for application layout, navigation, filters, forms, data tables, status displays, dialogs, feedback, device administration, session workflows, and later reporting features.

The repository already selects React and Leaflet for the dashboard. Leaflet remains the geospatial rendering engine and must not be replaced or obscured by a general-purpose UI component library.

## Decision

Use **React with TypeScript and Ant Design** as the RFx dashboard UI and design-system foundation.

Use Ant Design for:

- application shell, navigation, page layout, and responsive structure;
- forms, validation presentation, filters, and search controls;
- tables, pagination, sorting controls, and processing-status presentation;
- dialogs, drawers, notifications, confirmations, and error/empty/loading states;
- device, project, session, export, audit, and administrative workflows;
- shared RFx design tokens for color, typography, spacing, density, borders, and component behavior.

Use **React Leaflet/Leaflet** for maps, routes, markers, GeoServer layers, and MapProxy/OSM basemaps. Ant Design may provide surrounding panels and controls, but it does not own map rendering or geospatial state.

ASP.NET Core remains authoritative for tenant/project authorization. UI visibility and disabled controls improve usability but are not security boundaries.

## Boundaries

- Create a small RFx theme layer using supported Ant Design design tokens; avoid widespread one-off CSS overrides.
- Keep Leaflet lifecycle and map state isolated from ordinary Ant Design form/table state where practical.
- Provide reusable RFx components only when they encode repeated product behavior; do not wrap every Ant Design component automatically.
- Validate accessibility, keyboard operation, responsive layouts, map-control stacking, long labels, loading/error states, and supported-browser behavior.
- Validate performance with representative session tables and map datasets. Use server-side pagination/filtering and geospatial aggregation rather than sending unbounded data to the browser.
- Select the charting library through a representative RF/KPI prototype; Ant Design adoption does not automatically select a chart package.
- Do not use deprecated Create React App. Select and pin the build tool when dashboard initialization is approved.

## Consequences

The dashboard gains a coherent enterprise-oriented component system and reduces the need to design common interaction patterns from scratch. The team accepts Ant Design as a significant UI dependency and must manage version compatibility, theming, accessibility, bundle size, upgrade testing, and controlled customization.

Exact React, TypeScript, Ant Design, React Leaflet, Leaflet, Node.js, package-manager, build-tool, browser-support, and chart-library versions remain unapproved until the dashboard initialization milestone. Version selection must follow the repository toolchain policy and use pinned supported releases.

This decision does not move dashboard implementation into the first Android-to-PostGIS vertical slice. Dashboard work remains in the roadmap's later geospatial/dashboard phase.

