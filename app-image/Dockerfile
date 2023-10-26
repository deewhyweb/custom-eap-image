ARG BASEIMAGE=image-registry.openshift-image-registry.svc:5000/eap-test/eap-base-image
FROM $BASEIMAGE

ARG DEPLOYMENT_FILE=ROOT.war

ADD $DEPLOYMENT_FILE /deployments/ 
COPY setup/cli /tmp/cli

USER root
RUN chmod -R g+w /tmp
USER jboss

EXPOSE 8080
