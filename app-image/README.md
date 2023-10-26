oc apply -f openshift

oc start-build eap-app-image --from-dir=.
