pipeline {
  agent any
     
  stages {
    stage('Git Checkout') {
      steps {
        echo 'This stage is to clone the repo from github'
        git branch: 'master', url: 'https://github.com/krishnabolishetti/star-agile-health-care.git'
                        }
            }
    stage('Create Package') {
      steps {
        echo 'This stage will compile, test, package my application'
        sh 'mvn package'
                          }
            }
    
     stage('Create Docker Image') {
      steps {
        echo 'This stage will Create a Docker image'
        sh 'docker build -t krishnabolishetti/healthcare:1.0 .'
                          }
            }
   stage('Docker-Login') {
           steps {
            withCredentials([usernamePassword(credentialsId: 'dockervamc', passwordVariable: 'docker-Login', usernameVariable: 'docker-login')]) {
               sh 'docker login -u ${docker-login} -p ${docker-Login}'
                                   }
                        }
                }
    stage('Docker Push-Image') {
      steps {
        echo 'This stage will push my new image to the dockerhub'
        sh 'docker push cbabu85/healthcare:1.0'
            }
      }
    stage('AWS-Login') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'Awsaccess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
         }
      }
    }
   
    
  }
}
 
