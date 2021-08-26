def va_r
def dockerimg
pipeline {
	environment {
        registry = "rahulyerme1234/jenkinspipeline"
        registryCredential = 'dockerhub'
        }
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
  stage('Building docker image') {
      steps{
        script {
		
            dockerimg = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    } 
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

}
