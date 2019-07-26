# As per: https://access.redhat.com/solutions/3402171

FROM registry.redhat.io/redhat-sso-7/sso73-openshift:1.0-12.1561751916

MAINTAINER JA Bride

ENV ORACLE_HOST
ENV ORACLE_SERVICE_NAME
ENV ORACLE_USERNAME
ENV ORACLE_PASSWORD

# Add Oracle JDBC driver 
COPY /extensions /extensions

# Prepare for configuration
ENV DEFAULT_LAUNCH $JBOSS_HOME/bin/openshift-launch.sh
ENV DEFAULT_LAUNCH_NOSTART $JBOSS_HOME/bin/openshift-launch-nostart.sh
RUN cp $DEFAULT_LAUNCH $DEFAULT_LAUNCH_NOSTART
RUN sed -i '/^.*standalone.sh/s/^/echo/' $DEFAULT_LAUNCH_NOSTART
RUN sed -i '/^.*wait/s/^/echo/' $DEFAULT_LAUNCH_NOSTART
RUN $DEFAULT_LAUNCH_NOSTART

# Configure
RUN $JBOSS_HOME/bin/jboss-cli.sh --file=/extensions/actions.cli
RUN $DEFAULT_LAUNCH
