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
		}
      }
      stage('TEST') {           
        steps {
           parallel(
            Firefox: {
               // sleep 2
                bat "mvn test -Dbrowser=firefox"
                bat "echo Firefox"
            },
            InternetExplorer: {
                // sleep 2
                bat "mvn test -Dbrowser=ie"
                bat "echo IE"
            },
            Chrome: {
                // sleep 2
                bat "mvn test -Dbrowser=chrome"
                bat "echo Chrome"
            },            
            Edge: {
                // sleep 2
                bat "mvn test -Dbrowser=edge"   
                bat "echo Edge"    
            },
            Opera: {   
                // sleep 2
               bat "mvn test -Dbrowser=opera"
               bat "echo Opera"
            }
          )
        }
      } 
	    stage('BUILD') {
	     steps {
	        bat "mvn install"
           bat "echo Hi"
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
        // emailext body: '$DEFAULT_CONTENT', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'JENKINS: (${JOB_NAME}) (${BUILD_NUMBER}) : $DEFAULT_SUBJECT',to: 'vaichalkar.shailendra@gmail.com'
        echo "Mail Sent"
      }
    }
}
