pipeline{
  environment {
    registry = "shubhambharill1/devsecops-cyberfrat"
    registryCredential = "33898718-c2d8-4ddd-b834-f7762350fbf2"
    dockerImage =''
  }
  
  agent any 
    
  
  stages {

    stage ('secret scanning') {
    steps { 
      script { 
        sh " docker run trufflesecurity/trufflehog git --json https://github.com/shubham-bharill/CyberFRAT-DevSecOps-Training-Sample-Flask-App.git > trufflehog.json"
        sh "cat trufflehog.json"
          }
      }
    }
  
    stage ('Build Docker Image') {
      steps { 
        script { 
        dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('push to dockerhub') {
    steps {
      script {
        docker.withRegistry('',registryCredential) {
          dockerImage.push()
        }      
      }
    }
  } 

    
    stage ('test run'){
      steps {
        sh 'docker run -d  $registry:$BUILD_NUMBER'
      }

    }
  }
}
        
      
  
