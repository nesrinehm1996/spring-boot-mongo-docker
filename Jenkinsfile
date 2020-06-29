pipeline {
environment {
registry = "nesrinehm1996/spring-boot-mongo-docker"
registryCredential = 'dockerhub_id'
dockerImage = 'DOCKER_HUB_CREDENTIALS'
}
agent any
stages {
stage('Cloning our Git') {
steps {
git 'https://github.com/nesrinehm1996/spring-boot-mongo-docker.git'
}
}
stage('Building our image') {
steps{
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy our image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('Cleaning up') {
steps{
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
}
