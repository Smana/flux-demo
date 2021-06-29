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

## A typical dev workflow

Note: Not covering local dev for the moment

Here we'll consider we have to repostories:
* `apps`: where our application code lives
* `flux`: defines how is deployed our app

### Staging

* [flux]: If there are updates to add to secrets they must be done (using SOPS) before deploying the new app version.
* [flux]: Review staging Helm values if necessary (resources, replicas ...)
* [app]: Create a **pull-request** that contains the code changes in the application repository (here [gitops-demo](https://github.com/Smana/gitops-demo))
* [app]: The [CI](https://github.com/Smana/gitops-demo/actions/workflows/ci.yaml) steps are executed
* [app]: If ok the pull-request is merged. Merging triggers a job that pushes an new container image with a **tag** composed as follows `<stage>-<short_hash>-<timestamp>`
* [flux]: The image is **automatically updated** in the staging cluster using the timestamp to define the latest available tag

### Prod

Same as the staging except that we do not want to deploy the latest image tag. So we define explicitly the tag in the Helm values.