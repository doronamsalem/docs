# FLIGHT RAFAEL ![stronghold logo](https://www.google.com/imgres?imgurl=https%3A%2F%2Fyt3.googleusercontent.com%2Fytc%2FAL5GRJWxS5_ptaV_gozpraeHg4TnexR6H7PQTVAXktjtoQ%3Ds900-c-k-c0x00ffffff-no-rj&imgrefurl=https%3A%2F%2Fwww.youtube.com%2Fchannel%2FUC3D8NEM0cZQixoBfUIijHpQ&tbnid=2Q7sXHclVyEUVM&vet=12ahUKEwiY9eXYiM39AhUPlycCHSBzBhQQMygLegUIARCgAQ..i&docid=x_dfoBaKdUH6nM&w=900&h=900&q=rafael%20israel&hl=en&ved=2ahUKEwiY9eXYiM39AhUPlycCHSBzBhQQMygLegUIARCgAQ)

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
<p align="center">
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

### step 6

connect to argocd user with the rellevant permitions to the open shift cluster
under settings create:

* repository: connection of argo to gitlab repo 

<p align="center">
<img src="~/Pictures/argo-repository.png" alt="argo repository"
  width="686" height="289">
</p>

* project: create app of apps that deploy gitlab repo charts on cluster

<p align="center">
<img src="~/Pictures/argo-project.png" alt="argo project"
  width="686" height="289">
</p>
