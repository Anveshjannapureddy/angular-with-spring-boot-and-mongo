pipeline{
  agent any
  stages{
    stage("Build")
      {
      steps
      {
      echo 'running build automation'
      sh 'mvn clean install -DskipTests'
      archiveArtifacts artifacts: 'target/*.jar'
      }
    
      }
  }
}
