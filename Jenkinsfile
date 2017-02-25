node ('swarm-deploy') {
  
  artifactoryInput = "${env.APPS_COMPOSE}" /* Artifactory URL is input thru App Compose input */ 
  artifactoryList = artifactoryInput.tokenize(';')
  if ( artifactoryList.size() != 1 ) { 
    sh "echo Warning: This pipeline supports only one artifactory URL"
  }
  
  def ArtifactoryUrl = artifactoryList[0]
  sh "echo ${ArtifactoryUrl}"
  
  stage ('Download Stack Compose') {
    sh "curl -k -u ${env.ARTIFACTORY_USER}:${env.ARTIFACTORY_PASSWORD}  ${ArtifactoryUrl} -o ${env.JOB_NAME}.yml"
  }
    
  stage ('Deploy Docker App Bundle') {
    sh "docker stack deploy -c ${env.JOB_NAME}.yml ${env.JOB_NAME}" // deploy create as well as update stack - ?Does note seem to be working?
  }

  stage ('Publish Swarm Node and Service details') {
    sh "docker node ls"
    sh "docker stack ls"
    sh "docker service ls"
    sh "echo IP: `curl -s http://169.254.169.254/latest/meta-data/public-ipv4`"
  }
}
