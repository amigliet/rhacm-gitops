# Argo CD Resources

## 00appproj folder
The _00appproj_ folder contains all Argo CD AppProject resources that
are defined on every managed OpenShift GitOps instances.

## Overlays structure
Each folder is meant to be an overlay which collects all the resources
defined on a specific OpenShift GitOps instance.

Below a few structure examples.

* `<env>`
  ```
  $ mkdir -p {prod,stage}/projects/{default,myproject}/{app,appset}
  $ touch {prod,stage}/kustomization.yaml
  $ tree
  .
  ├── prod
  │   ├── kustomization.yaml
  │   └── projects
  │       ├── default
  │       │   ├── app
  │       │   └── appset
  │       └── myproject
  │           ├── app
  │           └── appset
  └── stage
      ├── kustomization.yaml
      └── projects
          ├── default
          │   ├── app
          │   └── appset
          └── myproject
              ├── app
              └── appset
  ```

* `<platform>`
  ```
  $ mkdir -p {azure,gcp}/projects/{default,myproject}/{app,appset}
  $ touch {azure,gcp}/kustomization.yaml
  $ tree
  .
  ├── azure
  │   ├── kustomization.yaml
  │   └── projects
  │       ├── default
  │       │   ├── app
  │       │   └── appset
  │       └── myproject
  │           ├── app
  │           └── appset
  └── gcp
      ├── kustomization.yaml
      └── projects
          ├── default
          │   ├── app
          │   └── appset
          └── myproject
              ├── app
              └── appset
  ```

* `<env>-<platform>`
  ```
  $ mkdir -p {prod,stage}-{azure,gcp}/projects/{default,myproject}/{app,appset}
  $ touch {prod,stage}-{azure,gcp}/kustomization.yaml
  $ tree
  .
  ├── prod-azure
  │   ├── kustomization.yaml
  │   └── projects
  │       ├── default
  │       │   ├── app
  │       │   └── appset
  │       └── myproject
  │           ├── app
  │           └── appset
  ├── prod-gcp
  │   ├── kustomization.yaml
  │   └── projects
  │       ├── default
  │       │   ├── app
  │       │   └── appset
  │       └── myproject
  │           ├── app
  │           └── appset
  ├── stage-azure
  │   ├── kustomization.yaml
  │   └── projects
  │       ├── default
  │       │   ├── app
  │       │   └── appset
  │       └── myproject
  │           ├── app
  │           └── appset
  └── stage-gcp
      ├── kustomization.yaml
      └── projects
          ├── default
          │   ├── app
          │   └── appset
          └── myproject
              ├── app
              └── appset
  ```
