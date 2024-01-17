# DevOps Journey Jenkins Course
---

This project is a fork from **[devopsjourney1/jenkins-101](https://github.com/devopsjourney1/jenkins-101)**

### Visit the DevopsJourney YouTube Link for the complete course lesson:
https://www.youtube.com/watch?v=6YZvp2GwT0A

#### Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)

```shell
docker build -t jenkins-blueocean:2.414.2 .
docker image tag jenkins-blueocean:2.414.2 alexmbarbosa/jenkins-blueocean:1.0
docker push alexmbarbosa/jenkins-blueocean:1.0
```

#### Create the network 'jenkins'

```shell
docker network create jenkins
```

#### Run the Container

```shell
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  alexmbarbosa/jenkins-blueocean:1.0
```

#### Get the Password

```shell
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

#### Connect to the Jenkins

```shell
https://localhost:8080/
```

* Installation Reference: [Installing Jenkins on Docker](https://www.jenkins.io/doc/book/installing/docker)


#### alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine

https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin

```shell
docker run -d --restart=always -p 127.0.0.1:2376:2375 \
  --network jenkins \
  -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock

docker inspect <container_id> | grep IPAddress
```

#### Using my Jenkins Python Agent

```shell
docker pull devopsjourney1/myjenkinsagents:python
```
All Credits for [DevOpsJourney](https://www.youtube.com/@DevOpsJourney) YouTube Channel
