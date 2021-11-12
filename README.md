# Red Hat Advanced Cluster Management and OpenShift GitOps

A flexible way to manage OpenShift Gitops instances with RHACM.

## Why this approach is worth considering
This repo is designed to enhance GitOps approach leveraged by OpenShift GitOps
in a multi-cluster environment with business continuity process managed by RHACM.

## How to use this repository
This is a boilerplate repository to quickly set up the initial resources. \
It provides:
  * Folder-based structure model
  * RHACM objects
  * OpenShift GitOps example objects

To start using this repo you can fork it and adapt it to your environment.

## Repository structure model
This is a GitOps folder-based structure model.

* [_rhacm-manifests_](rhacm-manifests): contains Red Hat Advanced Cluster Management manifests.
* [_gitops-manifests_](gitops-manifests)
  * [_operator_](gitops-manifests/operator): contains OpenShift GitOps operator's manifests.
  * [_resources_](gitops-manifests/resources): contains Argo CD resources, such as AppProjects, Applications and ApplicationSets.

### Why folder-based strategy and not branched-based
The folder-based approach has the following main advantages:
* The main branch is the Single Point of Truth for data
* Follows Git standard workflows
* Increase collaboration and the whole team enablement
* Avoids _independent_ branches proliferation
* Reduces time during troubleshooting

## Prerequisites
To use this repo you need to have in your enviroment:
* RHACM MultiClusterHub installed on an OpenShift Cluster.
* At least one OpenShift Cluster on which to install OpenShift Gitops operator.

## How to deploy resources on RHACM Hub
* Login into RHACM Hub cluster.
* Deploy this repository on RHACM:
  ```
  oc apply -k https://github.com/amigliet/rhacm-gitops/rhacm-manifests/overlays/stage
  ```

  This will create the following resources:
  * Channel
  * PlacementRule
  * Subscription for OpenShift GitOps operator configurations
  * Subscription for Argo CD resources (i.e. AppProjects, Applications, ApplicationSets)
  * Application

## Procedure to move OpenShift GitOps from Active to Backup OpenShift Cluster
First you need to have another OpenShift cluster on which you have installed
OpenShift GitOps operator. \
Then to switch from Active to Backup OpenShift cluster perform the following
steps on RHACM:
* Delete apps Subscription.
  * To maintain Argo CD resources of the Application - such as Deployments,
    Services, etc - set the `.spec.syncPolicy.preserveResourcesOnDeletion`
    value to `true` in the parent ApplicationsSet, as follow:
    ```
    kind: ApplicationSet
    spec:
      generators:
        - clusters: {}
      template:
        # (...)
      syncPolicy:
        # Don't delete Application's child resources, on parent deletion:
        preserveResourcesOnDeletion: true
    ```
  * See Argo CD documentation on [Application Deletion](https://argocd-applicationset.readthedocs.io/en/stable/Application-Deletion/).
* Edit PlacementRule to deploy Argo CD to Backup cluster.
* Wait until Argo CD server status is `Phase: Available` on Backup cluster.
* Redeploy apps Subscription.
