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
        
stage('Building/Deploying our image') { 
       steps
       {
             withCredentials([usernamePassword(credentialsId: 'cb799438-4019-41b1-826d-5ce2c4f53f10', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh ("docker login -u ${USERNAME} -p ${PASSWORD}")
                        sh ("docker build -t ${USERNAME}/testing:first .")
                        sh ("docker push ${USERNAME}/testing:first")
                }
     }
}
         
  stage("Deploy on AKS using cluster"){
              steps { 
    	            sshagent(['soumyadeep_1461679']){
    	            sh "scp -o StrictHostKeyChecking=no Spring.yaml soumyadeep_1461679@13.93.120.161:/home/soumyadeep_1461679/"
                   script
                          {
                                 try
                                 {
                              sh "ssh soumyadeep_1461679@13.93.120.161 kubectl apply -f Spring.yaml -n devops-soumyadeep-1461679"   
                                 }
                                 catch(error)
                                 {
                                 sh "ssh soumyadeep_1461679@13.93.120.161 kubectl create -f Spring.yaml -n devops-soumyadeep-1461679"   
                                 }
                   }
    	}
    }
  }
  }
}
