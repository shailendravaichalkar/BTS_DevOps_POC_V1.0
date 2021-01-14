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
              //sleep 10
              bat "mvn test -Dbrowser=firefox -Dheadless=false"
            },
            InternetExplorer: {
              // sleep 10
              bat "mvn test -Dbrowser=ie -Dheadless=false"
            },
            Opera: {
              // sleep 10
              bat "mvn test -Dbrowser=opera -Dheadless=false"
            },
            Edge: {
              // sleep 10
              bat "mvn test -Dbrowser=edge -Dheadless=false"
            },
            Chrome: {
              // sleep 10
              bat "mvn test -Dbrowser=chrome -Dheadless=false"
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
