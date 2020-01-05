pipeline 
{
   agent any

   stages
	{
	stage('checkout')
		{
   		 steps{
        		script{
           			 checkout scm
       			       }
    			}
		}
      stage('Build') 
		{
         	steps 
			{
           			 // Get some code from a GitHub repository
           			 git 'https://github.com/Anveshjannapureddy/angular-with-spring-boot-and-mongo'

           			 // Run Maven on a Unix agent.
           			 sh "mvn clean install -DskipTests"
				 archiveArtifacts artifacts: 'target/demo-0.0.1-SNAPSHOT.jar'

          
         		}

         		//post {
            			//success {
              				 //junit '**/target/surefire-reports/TEST-*.xml'
              				 //archiveArtifacts ''
           				// }
        			 //}
     		 }
	  stage('DeployToVm')
		{
		 //when
			//{
                		//branch 'master'
           		 //}
			steps 
			{
               			 withCredentials([usernamePassword(credentialsId: 'vm_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    		 sshPublisher(
                        			failOnError: true,
                        			continueOnError: false,
                        			publishers:[
                           				sshPublisherDesc(
                                				configName: 'deploy',
                               	 				sshCredentials:[
                                   	 				username: "$USERNAME",
                                    					encryptedPassphrase: "$USERPASS"
                                				], 
                                				transfers:[
                                   					 sshTransfer(
                                       	 					sourceFiles: 'target/demo-0.0.1-SNAPSHOT.jar',
                                        					removePrefix: 'target/',
                                        					remoteDirectory: '/tmp',
										execCommand: 'sudo stop demo-0.0.1-SNAPSHOT && sudo start demo-0.0.1-SNAPSHOT'
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
