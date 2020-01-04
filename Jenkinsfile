pipeline {
   agent any

   stages {
	stage('checkout'){
    steps{
        script{
            checkout scm
        }
    }
}
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/Anveshjannapureddy/angular-with-spring-boot-and-mongo'

            // Run Maven on a Unix agent.
            sh "mvn clean install -DskipTests"

          
         }

         post {
            success {
               //junit '**/target/surefire-reports/TEST-*.xml'
               archiveArtifacts 'target/*.jar'
            }
         }
      }
   }
}
