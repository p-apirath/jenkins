FROM jenkins/jenkins:2.164-alpine
# if we want to install via apk
USER root

# Update apk repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/main" > /etc/apk/repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

# Install the latest Docker CE binaries
RUN apk -U --no-cache --allow-untrusted add shadow 'go=1.11.5-r0' 'docker=18.09.1-r0' py-pip && pip install docker-compose
RUN usermod -aG docker jenkins

# Minimize size
RUN rm -rf /var/lib/apt/lists/* /var/cache/apk/* /usr/share/man /tmp/*

# drop back to the regular jenkins user - good practice
USER jenkins
