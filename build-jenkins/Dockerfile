FROM jenkins/jenkins:2.196-alpine
# if we want to install via apk
USER root
ENV SONAR_SCANNER_CLI_VERSION 4.1.0.1829

# Update apk repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/main" > /etc/apk/repositories && \
    echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    echo "http://dl-2.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \

# Install the latest Docker CE binaries
    apk update && \
    apk -U --no-cache --allow-untrusted add shadow docker ansible docker-compose kubernetes php maven go nodejs yarn npm py-pip py-setuptools

# Update pip and add jenkins to docker group
RUN pip install --upgrade pip

# Minimize size
RUN rm -rf /var/lib/apt/lists/* /var/cache/apk/* /usr/share/man /tmp/*

# Install Jenkins Plugins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/install-plugins.sh $(cat /usr/share/jenkins/plugins.txt)

# copy custom built plugins
# COPY plugins/*.hpi /usr/share/jenkins/ref/plugins/

# so we can use jenkins cli
COPY config/jenkins.properties /usr/share/jenkins/ref/

# remove executors in master
COPY config/*.groovy /usr/share/jenkins/ref/init.groovy.d/
# COPY config/master-executors.groovy /usr/share/jenkins/ref/init.groovy.d/

# lets configure Jenkins with some defaults
# COPY config/*.xml /usr/share/jenkins/ref/

# drop back to the regular jenkins user - good practice
# USER jenkins

# sonar-scanner-cli: https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/
RUN curl https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_CLI_VERSION}.zip -o sonar-scanner-cli-${SONAR_SCANNER_CLI_VERSION}.zip && \
    unzip sonar-scanner-cli-${SONAR_SCANNER_CLI_VERSION}.zip -d /opt/ && \
    ln -sf /opt/sonar-scanner-${SONAR_SCANNER_CLI_VERSION}/sonar-scanner /opt/sonar-scanner-${SONAR_SCANNER_CLI_VERSION}/sonar-scanner-debug /usr/bin && \
    rm -f sonar-scanner-cli-${SONAR_SCANNER_CLI_VERSION}.zip
