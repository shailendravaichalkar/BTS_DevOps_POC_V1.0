// Author : Shailendra V
// Jenkinsfile

pipeline {
   agent any
   tools ('Init') {
      maven "localMaven"
    }
   stages {
      stage('Build') {
		steps {
			git 'https://github.com/shailendravaichalkar/BTS_DevOps_POC_V1.0.git'
			bat "mvn clean compile"
		}
      }
      stage('test') {           
        steps {
           parallel(
            Firefox: {
               sleep 2
                bat "mvn test -Dbrowser=firefox"
            },
            InternetExplorer: {
                sleep 2
                bat "mvn test -Dbrowser=ie"
            },
            Chrome: {
                sleep 2
                bat "mvn test -Dbrowser=chrome"
            }
          )
        }
      } 
	    stage('package') {
	     steps {
	        bat "mvn install"
	     }
      }
      stage('Deploy in CERT') {
	     steps {
	        archiveArtifacts 'target/*.jar'
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
