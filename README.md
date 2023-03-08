# FLIGHT RAFAEL

Step by step guide for a version deployment.

## Prerequests

* Gitlab server
* JFrog Artifactory
* ArgoCD server 
* OpenShift
* Keycloak

## Assemption

* deployment performed from linux machine

## Automaticlly deployment
run <script.sh>

## Manual deployment

new version tar file  containing the following directories:
* dir name: docker images
* main: services cahrts 
* flight: chart that contains agrocd controllers per service 

### step 1

extrec new version tar file

```
tar -xf <new_v.tar>
```
<p>
<img src="https://github.com/doronamsalem/docs/blob/main/png/tar_example.png" alt="Extracted file example"
  width="686" height="289">
</p>
   
### step 2 

load all images to docker engine

```
docker load -i <service_image.tar>
```
<p>
<img src="https://github.com/doronamsalem/docs/blob/main/png/docker_load.png" alt="docker load for all images together"
  width="686" height="289">
</p>

### step 3

loaded images need to be tag according to local artifactory url

```
docker tag <rafael_artifactory>:<rafael_port>/service:tag  <local_artifactory>:<local_port>/service:tag
```

### step 3

push all tages images to artifactory

```
docker push <local_artifactory>:<local_port>/service:tag
```

### step 4

change the following in <flight>/values.yaml according to ...
* all endpoints
* ingressDomaine
* 

### step 5

* push to a clean gitlab repo the main dir charts to "main" branch
* on a differente branch of the same repo push the chart from <flight> dir 

### step 6:

connect to argocd user with the rellevant permitions to the open shift cluster
under settings create:

* project: new argo project 
* repository: connection of argo to gitlab repo using https

<p align="center">
<img src="https://github.com/doronamsalem/docs/blob/main/png/argo-repository.png" alt="argo repository"
  width="686" height="289">
</p>

### step 7

nevigate to application and create new app
GENERAL:
* SYNC POLICY: Automatic
  * PRUNE RESOURCES :heavy_check_mark:
  * SELF HEAL :heavy_check_mark:
SOURCE:
  * Revision: flight branch
  * Path: .
DESTINATION
  * Cluster URL: https://kubernetes.default.svc
  * Namespace: 

<p>
<img src="https://github.com/doronamsalem/docs/blob/main/png/app_of_apps.png" alt="app of apps"
  width="686" height="289">
</p>
