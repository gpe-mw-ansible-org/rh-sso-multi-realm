# As per: https://access.redhat.com/solutions/3402171
# Usage
#   1) cp /path/to/downloded/ojdbc8.jar extensions
#   2) sudo podman login registry.redhat.io -u xxx -p xxx
#   3) sudo podman build --rm -t rhsso-oracledb:1.0 .

FROM registry.redhat.io/redhat-sso-7/sso73-openshift:1.0-12.1561751916

MAINTAINER JA Bride

ENV ORACLE_HOST=bastion.example.com

ENV ORACLE_SERVICE_NAME ORCLPDB1.localdomain
ENV ORACLE_USERNAME rhsso
ENV ORACLE_PASSWORD rhsso

# Copy extensions directory to new image 
COPY /extensions /extensions
RUN sed -i "s/{ORACLE_HOST}/$ORACLE_HOST/g" /extensions/actions.cli
RUN sed -i "s/{ORACLE_SERVICE_NAME}/$ORACLE_SERVICE_NAME/g" /extensions/actions.cli
RUN sed -i "s/{ORACLE_USERNAME}/$ORACLE_USERNAME/g" /extensions/actions.cli
RUN sed -i "s/{ORACLE_PASSWORD}/$ORACLE_PASSWORD/g" /extensions/actions.cli

# Original shell script that (among other tasks) invokes standalone.sh
ENV DEFAULT_LAUNCH $JBOSS_HOME/bin/openshift-launch.sh

# Copy shell script and modify copy that that it does not invoke standalone.sh
ENV DEFAULT_LAUNCH_NOSTART $JBOSS_HOME/bin/openshift-launch-nostart.sh
RUN cp $DEFAULT_LAUNCH $DEFAULT_LAUNCH_NOSTART
RUN sed -i '/^.*standalone.sh/s/^/echo/' $DEFAULT_LAUNCH_NOSTART
RUN sed -i '/^.*wait/s/^/echo/' $DEFAULT_LAUNCH_NOSTART

# Execute modified shell script
RUN $DEFAULT_LAUNCH_NOSTART

# Execute additional CLI pertaining to use of Oracle RDBMS
RUN $JBOSS_HOME/bin/jboss-cli.sh --file=/extensions/actions.cli

# Invoke original shell script that invokes standalone.sh
RUN $DEFAULT_LAUNCH
