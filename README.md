# rhdh-community-plugins

Manifest-driven pipeline that exports Backstage community plugins as RHDH dynamic plugins.

## How it works

Edit `plugins.yaml` to add or update community plugins. A push triggers the pipeline which:

1. Clones the plugin source from [backstage/community-plugins](https://github.com/backstage/community-plugins)
2. Builds from source (`yarn install` + `yarn build:all`)
3. Exports as a dynamic plugin using the RHDH CLI
4. Packages as a public OCI image on quay.io
5. Updates `dynamic-plugins.yaml` in the platform repo
6. ArgoCD syncs → RHDH loads the new plugin without rebuilding

## Adding a community plugin

```yaml
# plugins.yaml
plugins:
  - name: <plugin-folder-name>      # e.g. github-actions
    workspace: <workspace-name>     # e.g. github-actions (in backstage/community-plugins/workspaces/)
    image: quay.io/edubois10/rhdh-plugin-<name>
    type: frontend | backend
```

Then configure `dynamic-plugins.yaml` in the platform repo with the OCI reference and pluginConfig.
