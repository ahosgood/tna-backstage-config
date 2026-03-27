# backstage-config

Templates and configuration files for the [National Archives](https://github.com/nationalarchives/) organisation to be used with [Backstage](https://github.com/backstage/backstage).

## Contents

### `app-config.yaml`

The main Backstage application configuration file. It wires up the GitHub integration, TechDocs, authentication, and points the Backstage catalog at the entity files in this repository.

Copy this file into the root of your Backstage app and update the environment variable placeholders (`GITHUB_TOKEN`, `AUTH_GITHUB_CLIENT_ID`, `AUTH_GITHUB_CLIENT_SECRET`) before starting the application.

### `catalog/`

Backstage catalog entity definitions for the services that make up the National Archives website.

| File | Kind | Description |
|------|------|-------------|
| `catalog/all.yaml` | `Location` | Root catalog entry; references all entity files below |
| `catalog/systems/tna-website.yaml` | `System` | The TNA website system |
| `catalog/components/ds-wagtail.yaml` | `Component` | [ds-wagtail](https://github.com/nationalarchives/ds-wagtail) – Django/Wagtail CMS |
| `catalog/components/ds-frontend.yaml` | `Component` | [ds-frontend](https://github.com/nationalarchives/ds-frontend) – Flask frontend application |
| `catalog/apis/wagtail-api.yaml` | `API` | Wagtail REST API (v2) provided by ds-wagtail and consumed by ds-frontend |

## Service relationships

```
ds-frontend  ──consumesApi──►  wagtail-api  ◄──providesApi──  ds-wagtail
     │                                                              │
     └─────────────────── system: tna-website ─────────────────────┘
```

## Adding this catalog to Backstage

Register `catalog/all.yaml` as a catalog location in your Backstage instance, either via the UI (**Catalog → Register existing component**) or by adding the following to `app-config.yaml`:

```yaml
catalog:
  locations:
    - type: url
      target: https://github.com/nationalarchives/backstage-config/blob/main/catalog/all.yaml
      rules:
        - allow: [Component, System, API, Location]
```