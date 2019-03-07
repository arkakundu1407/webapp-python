pipeline {
    environment {
     dockerImage = ''
   }
    agent none 
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python -m py_compile app.py' 
            }
        }
       stage('Building Image') {
         steps {
           script {
             dockerImage = docker.build registry + ":$BUILD_NUMBER"
             }
          }
       } 
    }
}
