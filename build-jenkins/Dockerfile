FROM jenkins/jenkins:2.177-alpine
# if we want to install via apk
USER root

# Update apk repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/main" > /etc/apk/repositories && \
    echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    echo "http://dl-2.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

# Install the latest Docker CE binaries
RUN apk -U --no-cache --allow-untrusted add shadow docker docker-compose kubernetes maven go nodejs py-pip py-setuptools && pip install --upgrade pip && usermod -aG docker jenkins

# drop back to the regular jenkins user - good practice
USER jenkins

# Install Jenkins Plugins
COPY plugins.txt /usr/share/jenkins/plugins.txt
#RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/install-plugins.sh /usr/share/jenkins/plugins.txt

# copy custom built plugins
COPY plugins/*.hpi /usr/share/jenkins/ref/plugins/

# so we can use jenkins cli
COPY config/jenkins.properties /usr/share/jenkins/ref/

# remove executors in master
COPY config/*.groovy /usr/share/jenkins/ref/init.groovy.d/

# lets configure Jenkins with some defaults
COPY config/*.xml /usr/share/jenkins/ref/

# Minimize size
RUN rm -rf /var/lib/apt/lists/* /var/cache/apk/* /usr/share/man /tmp/*
