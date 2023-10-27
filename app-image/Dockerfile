ARG BASEIMAGE=image-registry.openshift-image-registry.svc:5000/eap-test/eap-base-image@sha256:54ebee6de5ab3cd61a64cf03d28e0d1b77a60d7335a2c66902c01948adf6b467
FROM $BASEIMAGE

ARG DEPLOYMENT_FILE=ROOT.war

ADD $DEPLOYMENT_FILE /deployments/ 
COPY setup/cli /tmp/cli

USER root
RUN chmod -R g+w /tmp
USER jboss

EXPOSE 8080
