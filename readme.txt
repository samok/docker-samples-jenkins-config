##3Construction de l'image modifie avec docker dans une image de base jenkins
### Dockerfile
FROM jenkins/jenkins:lts

USER root
RUN apt-get update \
&& apt-get install curl ca-certificates \
&& curl -s https://get.docker.com/ | sed 's/docker-engine/docker-engine=%%DOCKER_VERSION%%*/' | sh \
&& echo 'DOCKER_OPTS=" -H :2375 -H unix:///var/run/docker.sock"' >> /etc/default/docker \
chmod 666 /var/run/docker.sock \
&& usermod -aG docker jenkins
USER jenkins

###modification des droits d'acces au docker.sock de l'hote
sudo chmod 666 /var/run/docker.sock

###lancement du container avec 2 volumes - 1 sur docker.sock de l'hote et 1 pour les datas de jenkins
### ouverture des ports 80 pour l'interface web et 50000 pour les slaves agents
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -d -ti jenkinswdocker:latest
(option --privileged peut etre ajoutee ?)

### DOCKER in DOCKER test
docker exec -ti 7bf52ef3d9b6 bash
docker ps
