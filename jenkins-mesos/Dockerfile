FROM jenkins:2.32.1

# Mesos plugin 0.14 requires native library 1.0.0
ARG MESOS_VERSION=1.0.0\*

# Mesos plugin needs mesos binary to be present
USER root
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF && \
    sh -c "echo deb http://repos.mesosphere.io/debian jessie main > /etc/apt/sources.list.d/mesosphere.list" && \
    apt-get update && \
    apt-get install -y mesos=$MESOS_VERSION && \
    rm -rf /var/lib/apt/lists/*

USER jenkins

# Disable executors
COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/executors.groovy

# Install mesos plugin and other useful plugins: pipeline, git, blueocean
RUN /usr/local/bin/install-plugins.sh mesos:0.14.0 \
  git:2.5.2 \
  workflow-aggregator:2.4 \
  blueocean:1.0.0-b14

ENTRYPOINT ["/bin/tini", "--", "/usr/local/bin/jenkins.sh"]

