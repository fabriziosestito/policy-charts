# Kubewarden policy-catalog

This repository contains the catalog of Kubewarden policies to be consumed by the Rancher UI. The catalog is published to GitHub Pages at [https://kubewarden.github.io/policy-catalog/](https://kubewarden.github.io/policy-catalog/).

## Release Policy Workflow

This repository includes a GitHub Action workflow for automatically updating policy references when new versions are released.

### Triggering the Workflow

The workflow can be triggered in two ways:

1. **Manually via workflow dispatch** - requires the following inputs:

   - `owner`: Repository owner
   - `repo`: Repository name
   - `tag`: Tag to release

2. **Automatically via repository dispatch** - requires this payload:
   ```json
   {
     "event_type": "release-policy",
     "client_payload": {
       "owner": "org-name",
       "repo": "repo-name",
       "tag": "v1.0.0",
       "artifacthub-pkg": "path/to/artifacthub-pkg.yml" # Optional, defaults to `./artifacthub-pkg.yml`
     }
   }
   ```

### What the Workflow Does

The workflow performs these steps:

- Fetches the policy repository at the specified tag
- Creates a directory structure under `charts/<repo-name>/`
- Copies required files (README.md, LICENSE)
- Copies optional files (questions-ui.yml → questions.yaml)
- Generates Chart.yaml using the pkg2chart tool
- Creates a PR with the changes

### Required Files

Each policy repository must contain the following:

- README.md
- LICENSE
- artifacthub-pkg.yml and artifacthub-repo.yml (for Chart.yaml generation)

Optional:

- questions-ui.yml (will be renamed to questions.yaml)
