def va_r
def dockerimg
pipeline {
    agent any
    tools{
     maven 'maven 3.6'
    }
   
 stages{
  stage("Version"){
   steps{
    script{
         va_r  = "${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
         echo "${va_r}"
    }
   }
  }
  stage('build ') {
      steps {
        script {
	
	         sh "mvn -s settings.xml -Drevision=$va_r clean package"	 
        }
      }
   }
 
  
  stage("Local Archive"){
   steps {
       archiveArtifacts artifacts: 'target/*.jar, target/*.war'
       }
    }
  stage('Deploy to nexus') {
    steps {
	    nexusArtifactUploader artifacts: 
		    [
		    [artifactId: 'jenkinstask', 
		     classifier: '', 
		     file: 'target/jenkinstask/0.0.1-SNAPSHOT.jar', 
		     type: 'war']
	            ], 
		    credentialsId: '45d4a7f7-1af5-48da-9bef-786b670e3ae4', 
		    groupId: 'com.jenkinstask', nexusUrl: '92.168.43.134', 
		    nexusVersion: 'nexus3', 
		    protocol: 'http', 
		    repository: 'simpleapp', 
		    version: '0.0.1-SNAPSHOT'
       
      }
    }
	 
 }
post {
    success {
       office365ConnectorSend color: '#00cc00', message: "Success  ${JOB_NAME} build_number:${BUILD_NUMBER}, branch:${BRANCH_NAME} url:(<${BUILD_URL}>)", status: 'SUCCESS', webhookUrl: "${TEAMS_WEBHOOK}"
    failure {
     office365ConnectorSend color: '#fc2c03', message: "Failed  ${JOB_NAME} build_number:${BUILD_NUMBER}, branch:${BRANCH_NAME} url:(<${BUILD_URL}>)", status: 'FAILED', webhookUrl:"${TEAMS_WEBHOOK}"
    }
  }
}
}
