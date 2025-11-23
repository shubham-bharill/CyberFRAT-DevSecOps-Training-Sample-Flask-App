pipeline{
  agent any 
  
  stages {
    stage ('Build Docker Image') {
      steps { 
        sh 'docker build -t cyberfrat:$BUILD_NUMBER .'
      }
    }
    stage ('test run'){
      steps {
        sh 'docker run -it cyberfrat:$BUILD_NUMBER'
      }

    }
  }
}
        
      
  
