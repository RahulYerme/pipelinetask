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
         va_r  = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
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
    archive '/var/lib/jenkins/jobs/Declarativetask*.war'
      
    }
    
   }
 }
}
