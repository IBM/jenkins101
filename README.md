# Jenkins 101

## Labs

- Lab01 - Create a Classic Pipeline with Jenkins (Ephemeral) on OpenShift 3, see [Lab01](lab-01/README.md)
- Lab02 - Create an OpenShift Pipeline with Jenkins Pipeline Build Strategy on OpenShift 3, see [Lab02](lab-02/README.md)


## OpenShift

A `Build` is the process to transform source code into a runnable image. A `BuildConfig` object is the definition of the entire build process. 

OpenShift Pipelines within your project, you will must use the Jenkins Pipeline Build Strategy. This strategy defaults to using a jenkinsfile at the root of your source repository

The `Build` strategies in OCP 3.11 are:
- Source-to-Image (S2I),
- Pipeline allows a Jenkins pipeline for execution by Jenkins,
- Docker,
- Custom.

The `Build` strategies in OCP 4.3 are:
- Docker build,
- Source-to-Image (S2I) build,
- Custom build,
- Pipeline build strategy can be used to implement sophisticated workflows for Continuous integration and Continuous deployment. Pipeline build strategy allows developers to define a Jenkins pipeline in a Jenkinsfile. However, it is deprecated on OCP 4 and is replaced with OpenShift Pipelines based on Tekton. 

With an `ImageStream`, you can trigger Builds and Deployments when a new image is pushed to the registry. You can configure Builds and Deployments to watch an image stream for notifications when new images are added and react by performing a Build or Deployment, respectively.
