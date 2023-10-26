ARG BASEIMAGE=image-registry.openshift-image-registry.svc:5000/eap-test/eap-base-image@sha256:bd639d1fae2e4324b02b74458dcf47944ab450f3953191807905d5ff20fb6be8
FROM $BASEIMAGE

ARG DEPLOYMENT_FILE=ROOT.war

ADD $DEPLOYMENT_FILE /deployments/ 
COPY setup/cli /tmp/cli

USER root
RUN chmod -R g+w /tmp
USER jboss

EXPOSE 8080
