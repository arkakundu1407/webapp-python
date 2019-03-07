pipeline {
    environment {
     dockerImage = ''
     containerId = sh(script: 'docker ps -aqf "name=python-app"',returnStdout: true)
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
