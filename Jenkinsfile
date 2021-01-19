// Author : Shailendra V
// Jenkinsfile

pipeline {
   agent any
   tools ('Init') {
      maven "localMaven"
    }
   stages {
      stage('FETCH CODE') {
		steps {
			git 'https://github.com/shailendravaichalkar/BTS_DevOps_POC_V1.0.git'
			bat "mvn clean compile"
         bat "echo code compilation Finished"
		}
      }
      stage('TEST') {           
        steps {
           parallel(
            Firefox: {
               // sleep 2
                bat "mvn test -Dbrowser=firefox"
                bat "echo Testing Completed in Firefox brower"
            },
            InternetExplorer: {
                // sleep 2
                bat "mvn test -Dbrowser=ie"
                bat "echo Testing Completed in IE browser"
            },
            Chrome: {
                // sleep 2
                bat "mvn test -Dbrowser=chrome"
                bat "echo Testing Completed in Chrome browser"
            },            
            Edge: {
                // sleep 2
                bat "mvn test -Dbrowser=edge"   
                bat "echo Testing Completed in Edge browser"    
            },
            Opera: {   
                // sleep 2
               bat "mvn test -Dbrowser=opera"
               bat "echo Testing Completed Opera browser"
            }
          )
        }
      } 
	    stage('BUILD') {
	     steps {
	        bat "mvn install"
           bat "echo Build Completed Successfully"
	     }
      }
      stage('CERT') {
	     steps {
           parallel(  
              Windows: {
                 echo "Windows"
              },
              UNIX: {
                 echo "Unix"
              }      
           )
	        // archiveArtifacts 'target/*.jar'
           // sshagent(['unix_devops']) {
              
           // }
         }
      }
            stage('PROD') {
	     steps {
           parallel(  
              Windows: {
                 echo "Windows Prod"
              },
              UNIX: {
                 echo "Unix Prod"
              }      
           )
	        // archiveArtifacts 'target/*.jar'
           // sshagent(['unix_devops']) {
              
           // }
         }
      }
	} 
	post {
      always {
        //emailext body: '$DEFAULT_CONTENT', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'JENKINS: (${JOB_NAME}) (${BUILD_NUMBER}) : $DEFAULT_SUBJECT',to: 'vaichalkar.shailendra@gmail.com'
        emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        echo "Mail Sent"
      }
    }
}
