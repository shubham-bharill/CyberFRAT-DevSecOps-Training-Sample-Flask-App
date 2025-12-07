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
    stage ('SAST'){
      steps {
        sh "rm -rf bandit.json || true"
        sh "bandit -r -f=json . -o=bandit.json || true"
        sh "cat bandit.json"
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
    stage ('deploy test application'){
      steps{
        sh 'docker stop flaskr && docker rm flaskr || true'
        sh "docker pull ${registry}:${BUILD_NUMBER}"
        sh "docker run -d -p 5000:5000 --name flaskr ${registry}:${BUILD_NUMBER}"
      }
    }

    stage ('dast') {
      steps{
        sh  "docker run --network host -t zaproxy/zap-stable zap-baseline.py -t 'http://localhost:5000' || true"

      }
    } 
  }
}
        
      
  
