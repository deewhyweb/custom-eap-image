ARG BASEIMAGE=image-registry.openshift-image-registry.svc:5000/eap-test/eap-base-image@sha256:715df6912609d3d5088fe8891a9701491a692c193cb96ff2d415c26b5b0bd3bd
FROM $BASEIMAGE

ARG DEPLOYMENT_FILE=ROOT.war

ADD $DEPLOYMENT_FILE /deployments/ 
COPY setup/cli /tmp/cli

USER root
RUN chmod -R g+w /tmp
USER jboss

EXPOSE 8080
