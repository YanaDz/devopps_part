jenkins configution for Docker
sourse:
https://www.jenkins.io/doc/book/installing/docker/

Steps (for Windows):
1. Create a bridge network in Docker
docker network create jenkins
2. Run a docker:dind Docker image
docker run --name jenkins-docker --rm --detach ^
  --privileged --network jenkins --network-alias docker ^
  --env DOCKER_TLS_CERTDIR=/certs ^
  --volume jenkins-docker-certs:/certs/client ^
  --volume jenkins-data:/var/jenkins_home ^
  --publish 2376:2376 ^
  docker:dind

3. Customise official Jenkins Docker image:
3.1 create Dockerfile
3.2 Build a new docker image from this Dockerfile and assign the image a meaningful name by command
docker build -t myjenkins-housekeeping .
4. Run your myjenkins image as a container in Docker using the following docker run command:
docker run --name myjenkins-housekeeping --restart=on-failure --detach ^
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 ^
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 ^
  --volume jenkins-data:/var/jenkins_home ^
  --volume jenkins-docker-certs:/certs/client:ro ^
  --publish 8090:8080 --publish 50000:50000 myjenkins-housekeeping

5. run jenkins in browser on
http://localhost:8090/
6. Insert Administrator password from logs of docker container
7. Start working

