/* Thie is my jenkins pipeline code  
    editing  this file for checking wehook */
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
        sh 'docker build -t krishnabolishetti/vamshi:1.0 .'
                          }
            }
   stage('Docker-Login') {
           steps {
           withCredentials([usernamePassword(credentialsId: 'dockercreds', passwordVariable: 'dockerpassword', usernameVariable: 'dockerlogin')]) {
               sh 'docker login -u ${dockerlogin} -p ${dockerpassword}'
                                   }
                        }
                }
    stage('Docker Push-Image') {
      steps {
        echo 'This stage will push my new image to the dockerhub'
        sh 'docker push krishnabolishetti/vamshi:1.0 '
            }
      } 
    stage('AWS-Login') {
      steps {
       withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'awslogin', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {

         }
      }
    }
     stage('setting the Kubernetes Cluster') {
      steps {
        dir('terraform_file'){
          sh 'terraform init'
          sh 'terraform validate'
          sh 'terraform apply --auto-approve'
          sh 'sleep 20'
        }
      }
    }
    stage('deploy kubernetes'){
steps{
  sh 'sudo chmod 600 ./terraform_file/key.pem'    
  sh 'minikube start'
  sh 'sleep 30' 
  sh 'sudo scp -o StrictHostKeyChecking=no -i ./terraform_file/key.pem deployment.yml ubuntu@172.31.29.116:/home/ubuntu/'
  sh 'sudo scp -o StrictHostKeyChecking=no -i ./terraform_file/key.pem service.yml ubuntu@172.31.29.116:/home/ubuntu/'
script{
  try{
  sh 'ssh -o StrictHostKeyChecking=no -i ./terraform_file/key.pem ubuntu@172.31.29.116 kubectl apply -f .'
  }catch(error)
  {
  sh 'ssh -o StrictHostKeyChecking=no -i ./terraform_file/key.pem ubuntu@172.31.29.116 kubectl apply -f .'
  }
}
}
}
    
  }
}
 
