pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "sdebnath13/testing"
   registryCredential = "cb799438-4019-41b1-826d-5ce2c4f53f10"
  }
  stages {
    stage('Initialize'){
      steps{
        echo "We are doing some test"
        echo "PATH = ${PATH}"
        }
    }
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
        
stage('Building our image') { 
             steps { 
                    script {
                 dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                 } 

     }
      stage('Deploy our image') { 
          steps { 
                 script { 
                  docker.withRegistry( '', registryCredential ) { 
                  dockerImage.push() 
                        }
                   } 
          }
      }
  }
}
