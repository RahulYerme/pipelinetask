def va_r
def dockerimg
pipeline {
	environment {
        registry = "rahulyerme1234/cicdpipelinetask"
        registryCredential = 'dockerhub'
        }
   agent any
    tools{
     maven 'Apache Maven 3.3.9'
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
  stage('BUILD') {
      steps {
        script {
	
	         sh "mvn clean package"	 
        }
      }
   }
 
  
  /*stage("Local Archive"){
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
		    credentialsId: 'newnexus', 
		    groupId: 'com.jenkinstask', 
		    nexusUrl: '192.168.33.10:8081', 
		    nexusVersion: 'nexus3',
		    protocol: 'http', 
		    repository: 'demorepo', 
		    version: '1.0.1-SNAPSHOT'
       
      }
    }*/
  stage('Downstream Job') {
      steps{
        script {
		build job: 'DeployJob',
		parameters: [
			 [$class: 'StringParameterValue', name: 'FROM_BUILD', value: "${BUILD_NUMBER}"],
		]
		
		
            
        }
      }
    } 
  /*stage("docker_scan"){
   steps{
        script {	  
        sh '''
         docker run -d --name db arminc/clair-db
         sleep 15
         docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan
	 
         sleep 1
         DOCKER_GATEWAY=$(docker network inspect bridge --format "{{range .IPAM.Config}}{{.Gateway}}{{end}}")
	 
         wget -qO clair-scanner https://github.com/arminc/clair-scanner/releases/download/v8/clair-scanner_linux_amd64 && chmod +x clair-scanner
	 
         ./clair-scanner --ip="$DOCKER_GATEWAY" --threshold="High" rahulyerme1234/cicdpipelinetask:$BUILD_NUMBER || exit 0
       '''
    }
   }
  }*/	  
	 
  stage('Deploy docker Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) 
	    {
		  
	    dockerimg.push("${env.BUILD_NUMBER}")
		  
            dockerimg.push("latest")
          }
        }
      }
    }
 }
 /*post {
    success {
      office365ConnectorSend color: '#00cc00', message: "Success  ${env.JOB_NAME} build_number:${env.BUILD_NUMBER}, branch:${env.GIT_BRANCH} url:(<${env.BUILD_URL}>)", status: 'SUCCESS', webhookUrl: "${env.TEAMS_WEBHOOK}"
    }
    failure {
      office365ConnectorSend color: '#fc2c03', message: "Failed  ${env.JOB_NAME} build_number:${env.BUILD_NUMBER}, branch:${env.GIT_BRANCH} url:(<${env.BUILD_URL}>)", status: 'FAILED', webhookUrl:"${env.TEAMS_WEBHOOK}"
    }*/
  }

