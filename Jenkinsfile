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
                  bat 'copy target\\*.jar c:\\POC\\'
                  echo "CERT Windows Tier Deployment is completed"
               },
               UNIX: {
                  build 'BTS_MavenSelenium_POC_v1.0_toUnix_CERT'
                  echo "CERT Unix Tier Deployment is completed"
              }      
           )
         }
      }

      stage('PROD') {
	      steps {
            def userInput = false
            script {
               def userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
               echo 'userInput: ' + userInput
               if(userInput == true) {
                  echo "HI"
                // do action
               } else {
                // not do action
                echo "PRODUCTION Deployment is NOT Done."
               }
            }    
	      }
      } 
	   
      post {
         always {
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                 subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                 to: "vaichalkar.shailendra@gmail.com"
            echo "Mail Sent"
         }
      }
   }
}
