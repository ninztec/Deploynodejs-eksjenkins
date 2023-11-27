pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_login', url: 'https://github.com/ninztec/Deploynodejs-eksjenkins.git']]])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t rjshk013/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_pwd')]) {
                    sh 'docker login -u devopshint -p ${dockerhub_pwd}'
                 }  
                 sh 'docker push rjshk013/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "nodeapp-deployment-service.yml", kubeconfigId: "kubernetes_config")
        }
      }
    }

  }
}
