# Jenkins 101

## Prerequirements

For this lab you need the following setup:

1. An IBM Cloud account, for instructions go [here](https://ibm.github.io/workshop-setup/NEWACCOUNT/).
1. Access to an OpenShift 4.5 cluster, for an IBM managed workshop you can add an existing OpenShift cluster to your IBMId, for instructions go [here](https://ibm.github.io/workshop-setup/GRANTCLUSTER/).
1. A client terminal at CognitiveClass.ai, for instructions go [here](https://ibm.github.io/workshop-setup/COGNITIVECLASS/).
1. To connect to a RedHat OpenShift Kubernetes Service (ROKS), follow the instructions [here](https://ibm.github.io/workshop-setup/ROKS/).

## Labs

* [Lab01](lab-01/README.md), Create a Classic Pipeline with Jenkins (Ephemeral) on OpenShift 4.5.
* [Lab02](lab-02/README.md), Create an OpenShift Pipeline with Jenkins Pipeline Build Strategy on OpenShift 4.5.

## OpenShift

A `Build` is the process to transform source code into a runnable image. A `BuildConfig` object is the definition of the entire build process.

The `Jenkins Pipeline Build Strategy` defaults to using a jenkinsfile at the root of your source repository.

The `Build` strategies in OCP 4.5 are:

* Source-to-Image (S2I),
* Pipeline allows a Jenkins pipeline for execution by Jenkins,
* Docker,
* Custom.

The `Build` strategies in OCP 4.5 are:

* Docker build,
* Source-to-Image (S2I) build,
* Custom build,
* Pipeline build, can be used to implement sophisticated workflows for Continuous Integration and Continuous Deployment (CI/CD). Pipeline build strategy allows developers to define a Jenkins pipeline in a Jenkinsfile. **Note** the Pipeline build strategy is [deprecated in OCP 4](https://docs.openshift.com/container-platform/4.5/builds/build-strategies.html#builds-strategy-pipeline-providing-jenkinsfile_build-strategies) and is replaced with OpenShift Pipelines based on Tekton.

With an `ImageStream`, you can trigger Builds and Deployments when a new image is pushed to the registry. You can configure Builds and Deployments to watch an image stream for notifications when new images are added and react by performing a Build or Deployment, respectively.

## Contributors

* Remko de Knikker [github](https://github.com/remkohdev)
