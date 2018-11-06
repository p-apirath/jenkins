FROM jenkins/jenkins:2.141
# if we want to install via apt
USER root
# RUN apt-get update && apt-get install -y ruby make more-thing-here
RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install -y apt-utils
RUN apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable"
RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install -y docker-ce
#RUN groupadd docker
RUN usermod -aG docker jenkins
# drop back to the regular jenkins user - good practice
USER jenkins
