# Flux (v2) Demo

Demonstrate how we could use a complete GitOps workflow:

* Deploy helm releases
* Images update automation in staging
* Deploy the monitoring stack
* Secrets management with Sops

## Directory structure
|                |               |                | Description                                                          |
| -------------- | ------------- | -------------- | -------------------------------------------------------------------- |
| apps           |               |                |                                                                      |
|                | base          |                | Base application definition (for all environments/clusters)          |
|                |               | helloworld     | A Helmrelease example                                                |
|                | staging       |                |                                                                      |
|                |               | helloworld     | Override values for staging env                                      |
| clusters       |               |                |                                                                      |
|                | prod          |                | Flux configuration for production clusters                           |
|                |               | europe         |                                                                      |
|                |               | us             |                                                                      |
|                | staging       |                | Flux configuration for the staging cluster                           |
|                |               | flux-system    |                                                                      |
|                |               | images-updates | How the images are automatically updated when a new tag is available |
| infrastructure |               |                |                                                                      |
|                | cert-manager  |                | Example of an infrastructure helm release                            |
|                | images        |                | Container images for image update automation                         |
|                | monitoring    |                |                                                                      |
|                |               | config         | Configuration of the monitoring (prometheus operator resources)      |
|                |               | stack          | Monitoring base stack (kube-prometheus)                              |
|                | notifications |                | Example of notifications (Github)                                    |
|                | secrets       |                | Infrastructure secrets using SOPS                                    |
|                | sources       |                | Different sources definition (Helm, Git ...)                         |