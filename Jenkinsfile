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
     
     stage ('Deploy application') {
       steps {
           KubernetesDeploy(
               kubeconfig : 'kubeconfig',
               configs : 'Application.yml',
               enableConfigSubstitution : false
           )
       }
     }
    }
}
