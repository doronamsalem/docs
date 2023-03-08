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

## Deployment

new version tar file  containing the following directories:
* <dir name>: docker images
* main: services cahrts 
* flight: chart that contains agrocd controllers per service 

###step 1

extrec new version tar file
```
tar -xf <new_v.tar>
```
<p align="center">
  
</p>
<p align="center">
<img src="~/Pictures/tar_example.png" alt="Extracted file example"
  width="686" height="289">
</p>
   
###step 2 


   
   
   
   
   
   
   
   
   image of extracted tar file with tree view of all files
   
   



### Executing program

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Authors

Contributors names and contact info

ex. Dominique Pizzie  
ex. [@DomPizzie](https://twitter.com/dompizzie)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Inspiration, code snippets, etc.
* [awesome-readme](https://github.com/matiassingers/awesome-readme)
* [PurpleBooth](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
* [dbader](https://github.com/dbader/readme-template)
* [zenorocha](https://gist.github.com/zenorocha/4526327)
* [fvcproductions](https://gist.github.com/fvcproductions/1bfc2d4aecb01a834b46)
