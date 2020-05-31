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

1. Delete the existing BuildConfig,
```
$ oc delete buildconfig spring-client-pipeline
buildconfig.build.openshift.io "spring-client-pipeline" deleted
```

## Deploy the Spring Client

1. Create the project,
```
$ oc new-project $PROJECT
```

1. Create the BuildConfig declaration file,
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

1. Create the BuildConfig and the OpenShift Pipeline,
```
$ oc create -f spring-client-pipeline.yaml
buildconfig.build.openshift.io/spring-client-pipeline created
```

1. Start the pipeline,
```
$ oc start-build spring-client-pipeline
build.build.openshift.io/spring-client-pipeline-1 started
```
