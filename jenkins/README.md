### How to run it

`docker build -t  jenkins-custom .`

`docker run --name jenkins-custome --network jenkins-net -d \
 -u root -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock \
 -v jenkins-d:/var/jenkins_home \
 jenkins-custom:latest`