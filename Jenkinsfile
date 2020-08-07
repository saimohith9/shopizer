pipeline{
environment{
registry = "saimohith9/shopizer"
registryCredentials = 'docker_hub'
dockerImage = ''
}
agent any
stages{
stage('Build'){
steps{
  sh 'mvnw clean install'
}
}
stage('Archive'){
steps{
archiveArtifacts '**/*.jar'
}
}
stage('Build Docker Image'){
steps{
script{
dockerImage = docker.Build registry + ":$BUILD_NUMBER"
}
}
}
stage('Push Docker Image'){
steps{
script{
docker.withRegistry( '', registryCredentials)
dockerImage.push()
dockerImage.push('latest')
}
}
}
stage('Deploy to Dev'){
steps{
  sh 'docker rm -f shopizer || true'
  sh 'docker run -d --name=shopizer -p 8081:8080 saimohith9/shopizer'
}
}
}
}
