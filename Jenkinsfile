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
                                    						    )
                                					  ]
                           	 				         )
                        			
							   ]
					 )
					 sshPublisher(
						 publishers: [
							 sshPublisherDesc(
								 configName: 'deploy', 
								 sshCredentials: [encryptedPassphrase: '{AQAAABAAAAAQRC7bWS4bV+S7Nt+gxwpXlAN2G0uoTacEi1gUlI1ZJjY=}',
								 key: '', keyPath: '', username: 'anvesh'], 
								 transfers: [sshTransfer(cleanRemote: true, excludes: '', 
								execCommand: 'java -jar /tmp/demo-0.0.1-SNAPSHOT.ja', 
								execTimeout: 120000, 
								flatten: false, makeEmptyDirs: false, 
								noDefaultExcludes: false, patternSeparator: '[, ]+',
								remoteDirectory: '/tmp', remoteDirectorySDF: false, 
								removePrefix: 'target', 
								sourceFiles: 'target/demo-0.0.1-SNAPSHOT.jar')], 
								usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                   	 						   
				
                	}
            	}
        }
	
}
}
