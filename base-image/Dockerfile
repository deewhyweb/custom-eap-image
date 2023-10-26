FROM registry.redhat.io/jboss-eap-7/eap74-openjdk11-openshift-rhel8

ENV GALLEON_PROVISION_LAYERS=cloud-server,ejb,jsf,h2-driver

RUN /usr/local/s2i/assemble

EXPOSE 8080 8443 443

ARG POSTGRES_JDBC_JAR=postgresql-42.6.0.jar

ARG POSTGRES_JDBC_URL=https://jdbc.postgresql.org/download/${POSTGRES_JDBC_JAR}

ARG BIN_PATH=$JBOSS_HOME/bin

# Download jdbc-drivers

ADD scripts /tmp/scripts

ADD modules /opt/eap/modules

USER root
RUN curl -s $POSTGRES_JDBC_URL -o /tmp/${POSTGRES_JDBC_JAR}

RUN cp /tmp/${POSTGRES_JDBC_JAR} /opt/eap/modules/org/postgresql/main

# Concat & apply cli scripts and delete standalone.xml history
RUN echo "embed-server --server-config=standalone-openshift.xml --std-out=echo" > /tmp/setup.cli \
    && cat /tmp/scripts/cli/*.cli >> /tmp/setup.cli
RUN $BIN_PATH/jboss-cli.sh --file=/tmp/setup.cli \
    && rm -rf /opt/eap/standalone/configuration/standalone_xml_history

# cli creates some new folders, add permissions for root group to allow random OpenShift User to access
USER root
RUN chmod -R g+w /opt/eap/standalone/tmp /opt/eap/standalone/data /opt/eap/standalone/log
USER jboss

# copy server to (smaller) runtime image:
FROM registry.redhat.io/jboss-eap-7/eap74-openjdk11-runtime-openshift-rhel8

#Disable embedded messae broker:
ENV DISABLE_EMBEDDED_JMS_BROKER=true

COPY --from=0 /opt/eap $JBOSS_HOME

# add postconfigure.sh, which will run cli scripts included in app image for runtime configuration
COPY scripts/postconfigure.sh /opt/eap/extensions/postconfigure.sh

USER root
RUN chmod 774 /opt/eap/extensions/*.sh && chown -R jboss:root $JBOSS_HOME && chmod -R ug+rwX $JBOSS_HOME

RUN ln -s $JBOSS_HOME/standalone/deployments /deployments

# enable parameter values in cli commands.  Needed to enable use of env variables for cli config.
RUN sed -i "s/<resolve-parameter-values>false<\/resolve-parameter-values>/\
<resolve-parameter-values>true<\/resolve-parameter-values>/" \
$JBOSS_HOME/bin/jboss-cli.xml

USER jboss
CMD $JBOSS_HOME/bin/openshift-launch.sh
