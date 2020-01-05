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
			 archiveArtifacts artifacts: 'dist/sampleapplication.zip'

          
         }

         //post {
            //success {
               //junit '**/target/surefire-reports/TEST-*.xml'
               //archiveArtifacts ''
           // }
         //}
      }
	  stage('DeployToVm'){
		 when {
                branch 'master'
            }
			steps {
                withCredentials([usernamePassword(credentialsId: 'vm_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deploy',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/sampleapplication.zip',
                                        //removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop sampleapplication && rm -rf /opt/sampleapplication/* && unzip /tmp/sampleapplication.zip -d /opt/sampleapplication && sudo /usr/bin/systemctl start sampleapplication'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
	  
	  }
   }
}
