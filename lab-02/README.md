# Lab02 - Create an OpenShift Pipeline with Jenkins Pipeline Build Strategy on OpenShift 3

See: https://docs.openshift.com/container-platform/3.11/dev_guide/dev_tutorials/openshift_pipeline.html

OpenShift Pipelines give you control over building, deploying, and promoting your applications on OpenShift. Using a combination of the Jenkins Pipeline Build Strategy, Jenkinsfiles, and the OpenShift Domain Specific Language (DSL) (provided by the OpenShift Jenkins Client Plug-in), you can create advanced build, test, deploy, and promote pipelines for any scenario. 

The OpenShift Jenkins Client Plug-in must be installed on your Jenkins master. The OpenShift Jenkins Client Plug-in provides a fluent-styled DSL for communicating with the OpenShift API from within the Jenkins slaves. The OpenShift DSL is based on Groovy syntax and provides methods for controlling the lifecycle of your application such as create, build, deploy, and delete. This strategy defaults to using a jenkinsfile. The jenkinsfile is executed on the Jenkins slave pod.

This lab creates an OpenShift Pipeline that will build, deploy, and verify a Node.js/MongoDB application using the nodejs-mongodb.json template.

1. Create a BuildConfig,
1. Create a file named `nodejs-sample-pipeline.yaml`,

    ```
    $ echo 'kind: "BuildConfig"
    apiVersion: "v1"
    metadata:
    name: "nodejs-sample-pipeline"
    spec:
    strategy:
        jenkinsPipelineStrategy:
        jenkinsfile: <pipeline content from below>
        type: JenkinsPipeline' > nodejs-sample-pipeline.yaml
    ```

```
$ PROJECT=springclient-ns
$ oc delete project $PROJECT
$ oc delete buildconfig spring-client-pipeline
buildconfig.build.openshift.io "spring-client-pipeline" deleted

project.project.openshift.io "springclient-ns" deleted
$ oc new-project $PROJECT

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

$ oc create -f spring-client-pipeline.yaml
buildconfig.build.openshift.io/spring-client-pipeline created
$ oc start-build spring-client-pipeline
build.build.openshift.io/spring-client-pipeline-1 started






