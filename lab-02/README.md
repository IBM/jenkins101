# OpenShift Pipeline with Pipeline Strategy on OpenShift 3

See: https://docs.openshift.com/container-platform/3.11/dev_guide/dev_tutorials/openshift_pipeline.html

You can use OpenShift Pipelines to build, test, deploy, and promote your applications on OpenShift. This lab creates an OpenShift Pipeline that will build, deploy, and verify a Node.js/MongoDB application using the nodejs-mongodb.json template.

1. From the IBM Cloud cluster dashboard, open the `OpenShift web console`,
1. In the `OpenShift web console`, from the Profile drowndown, click the `Copy Login Command`,
1. In the `cloud shell`, paste the login command to connect,

```
oc login https://c107-e.us-south.containers.cloud.ibm.com:31608 --token=FGg1_K2V3cnUdRtmtccBg4Z5VEdnfu6yZ7nO8mcO1-2
```

