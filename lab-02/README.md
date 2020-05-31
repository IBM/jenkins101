# Lab02 - Create an OpenShift Pipeline with Jenkins Pipeline Build Strategy on OpenShift 3

See: https://docs.openshift.com/container-platform/3.11/dev_guide/dev_tutorials/openshift_pipeline.html

OpenShift Pipelines give you control over building, deploying, and promoting your applications on OpenShift. Using a combination of the Jenkins Pipeline Build Strategy, Jenkinsfiles, and the OpenShift Domain Specific Language (DSL) (provided by the OpenShift Jenkins Client Plug-in), you can create advanced build, test, deploy, and promote pipelines for any scenario. 

The OpenShift Jenkins Client Plug-in must be installed on your Jenkins master. The OpenShift Jenkins Client Plug-in provides a fluent-styled DSL for communicating with the OpenShift API from within the Jenkins slaves. The OpenShift DSL is based on Groovy syntax and provides methods for controlling the lifecycle of your application such as create, build, deploy, and delete. This strategy defaults to using a jenkinsfile. The jenkinsfile is executed on the Jenkins slave pod.

This lab creates an OpenShift Pipeline that will build, deploy, and verify a Node.js/MongoDB application using the nodejs-mongodb.json template.

## Clean up Existing Deployment

1.Delete the existing project,
```
$ PROJECT=springclient-ns
$ oc delete project $PROJECT
project.project.openshift.io "springclient-ns" deleted
```


## Deploy the Spring Client

1. Create the project anew,
```
$ oc new-project $PROJECT
```

1. Create the BuildConfig declaration file using a Jenkins pipeline strategy,
```
$ echo 'kind: "BuildConfig"
apiVersion: "v1"
metadata:
  name: "spring-client-pipeline"
  namespace: springclient-ns
spec:
  source:
    git:
      uri: "https://github.com/remkohdev/spring-client"
      ref: "master"
  strategy:
    jenkinsPipelineStrategy:
      jenkinsfilePath: Jenkinsfile-oc
      env:
        - name: "LOGIN_URL"
          value: "https://c107-e.us-south.containers.cloud.ibm.com"
        - name: "LOGIN_PORT"
          value: "31608"
        - name: "PROJECT"
          value: "springclient-ns"' > spring-client-pipeline.yaml 
```

1. A BuildConfig or build configuration describes a build definition and triggers for when a new build should be created. The Pipeline build strategy allows you to define a Jenkins pipeline for execution by the Jenkins pipeline plugin. The first time a project defines a build configuration using a Pipeline strategy, OpenShift Container Platform instantiates a Jenkins server to execute the pipeline. The first time a user creates a build configuration using the Pipeline build strategy, OpenShift Container Platform looks for a template named `jenkins-ephemeral` in the openshift namespace and instantiates it within the user’s project. 

1. Review the Jenkinsfile for the OpenShift Pipeline at https://github.com/remkohdev/spring-client/Jenkinsfile-oc. The pipeline is using the [Maven DSL](https://jenkinsci.github.io/job-dsl-plugin/#path/freeStyleJob-steps-maven) (Domain Specific Language) and the OpenShift DSL.

1. Create the BuildConfig and the OpenShift Pipeline,
```
$ oc create -f spring-client-pipeline.yaml
buildconfig.build.openshift.io/spring-client-pipeline created
```

1. Go to the `Application Console`,
1. Go to the `springclient-ns` project,
1. Go to the Overview,

    ![OpenShift Overview](../images/jenkins-deployment-config.png)

1. A Jenkins instance was created in the new project namespace,
1. Go to Builds > Pipelines
1. Click `Start Pipeline`,

    ![OpenShift Jenkins start pipeline](../images/jenkins-start-pipeline.png)

1. Or from the cloud shell start a build of the pipeline,

```
$ oc start-build spring-client-pipeline
build.build.openshift.io/spring-client-pipeline-1 started
```

1. A pipeline build was started,

    ![OpenShift Jenkins pipeline started](../images/jenkins-pipeline-started.png)

1. To review the pipeline in the Jenkins Dashboard, go to Overview,

    ![OpenShift Overview](../images/openshift-applications-overview.png)

1. Click the Route for the `jenkins-ephemeral` instance,

    ![Jenkins Log in with OpenShift](../images/jenkins-login-with-openshift.png)

1. Click `Log in with OpenShift`,

    ![Jenkins Dashboard](../images/jenkins-dashboard-1.png)

1. Click the `Open Blue Ocean` link,

    ![Jenkins Blue Ocean](../images/jenkins-open-blue-ocean.png)

1. The `Open Blue Ocean` dashboard lists the new pipeline,

    ![Open Blue Ocean pipelines](../images/open-blue-ocean-pipelines.png)

1. Click on the `springclient-ns/springclient-ns/spring-client-pipeline` pipeline,
1. The builds for the pipeline are listed,

    ![Open Blue Ocean pipelines](../images/open-blue-ocean-pipeline-builds.png)

1. Click on build number 1,

    ![Open Blue Ocean pipelines](../images/open-blue-ocean-pipeline-build-1.png)

