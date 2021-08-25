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
			     file: 'target/jenkinstask-1.0.1-SNAPSHOT.jar', 
			     type: 'jar']], 
		    credentialsId: 'de5e57d0-3c8c-4e34-a781-3671dadd6b15', 
		    groupId: 'com.jenkinstask', 
		    nexusUrl: '192.168.43.134:8081', 
		    nexusVersion: 'nexus3',
		    protocol: 'http', 
		    repository: 'demorepo', 
		    version: '1.0.1-SNAPSHOT'
       
      }
    }
	 
 }
post {
    success {
        office365ConnectorSend color: '#00cc00', message: "Success  ${JOB_NAME} build_number:${BUILD_NUMBER}, branch:${env.GIT_BRANCH} url:(<${BUILD_URL}>)"
    failure {
    office365ConnectorSend color: '#fc2c03', message: "Failed  ${JOB_NAME} build_number:${BUILD_NUMBER}, branch:${env.GIT_BRANCH} url:(<${BUILD_URL}>)"
    }
  }
}
}
