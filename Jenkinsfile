pipeline {
    environment {
     registry = "arkakundu1407/docker-pipeline"
     registryCredential = 'dockerhub'
     dockerImage = ''
     containerId = sh(script: 'docker ps -aqf "name=node-app"',returnStdout: true)
   }
    agent any
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
        stage('Sonar Sacnner')
        {
            steps {  
           sh 'sonar-scanner \
  -Dsonar.projectKey=python-app \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://13.71.122.102:9000 \
  -Dsonar.login=e97f8e5bbdd95ea1e82876c0ab3702536cb037f1'
            
            }  
        }
        stage('Building Image') {
         steps {
           script {
             dockerImage = docker.build registry + ":$BUILD_NUMBER"
             }
          }
       }
     stage('Push Image') {
       steps{
         script{
           docker.withRegistry('',registryCredential) {
             dockerImage.push()
         }
        }
      }
     }
     
     stage ('Run container') {
       steps {
          sh 'docker run --name=node-app -d -p 8082:80 $registry:$BUILD_NUMBER &'
       }
     }
    }
}
