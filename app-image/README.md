oc apply -f openshift

oc start-build eap-app-image --from-dir=.



# To use pipelines

Install the OpenShift Pipelines operator

`oc apply -f pipelines-operator.yml`

Deploy maven, git-clone, and openshift-client tekton modules

`oc apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/maven/0.2/maven.yaml`

`oc apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.8/git-clone.yaml`

`oc apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/openshift-client/0.2/openshift-client.yaml`