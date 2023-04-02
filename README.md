# FLIGHT RAFAEL

Step by step guide for a version deployment.

## Prerequisites

* Git repository server
* ArgoCD server
* Docker images registry
* OpenShift cluster
* Keycloak server

## Assumption

* deployment performed from a Linux machine
* 60 GB free disk space

## Deployment

The application version is delivered as a tar file containing the following:
* “images” directory - contains all docker images in tar format
* “main” directory - contains charts of all the application services
* App-Of-Apps directory (mentioned as “AOA” in the guide)- chart with:
  * templates: ArgoCD controller for every service
  * values.yaml: environment variables 
  * Chart.yaml: chart configuration file 

### Step 1

Extract the tar file

```
tar -xf <new_v.tar>
```

### Step 2

Load all images from the directory “images” to docker-engine

```
docker load -i <service_image.tar>
```

### Step 3

All images are tagged according to Rafael’s environment 
loaded images can be displayed with

```
docker images
```

retagged all images according to the local docker registry URL

```
docker tag <rafael_artifactory>:<rafael_port>/service:tag <local_registry>:<local_port>/service:tag
```

### Step 4

Push all tagged images to the registry

```
docker push <local_registry>:<local_port>/service:tag
```

### Step 5

Change the following parameters in AOA/values.yaml file
* all endpoints
* replicaCount as necessary
```
ingressDomain: "<domain>"
```

### Step 6

Push to a clean Git repository all the charts from the main directory  to a "main" branch
to a different branch of the same repository push the chart content from the AOA directory

### Step 7

Connect the Keycloak admin dashboard 
* Define client id  to the relevant services
* Define users

### Step 8

Connect to the ArgoCD user with the relevant permissions to the open shift cluster
under settings, create:
* repository: connection of ArgoCD to the Git repository
* project: new ArgoCD project
  * SOURCE REPOSITORIES: git repository configured
  * DESTINATIONS: namespaces where argocd AOA and services applications will deploy (e.g: argo-cd and flight)

### Step 9

Navigate to the application and create a new app

* GENERAL:
  * Project: Name: the project that was created in step 8
  * SYNC POLICY: Automatic
  * PRUNE RESOURCES :heavy_check_mark:
  * SELF HEAL :heavy_check_mark:
   
* SOURCE:
  * Revision: the branch that the AOA chart was pushed to
  * Path  ./

* DESTINATION
  * Cluster URL:  in-cluster (ArgoCD will deploy in the same cluster it deployed)
  * Namespace: the namespace where ArgoCD is deployed in your cluster
