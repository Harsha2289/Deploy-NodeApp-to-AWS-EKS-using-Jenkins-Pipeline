pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/raushan8586/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
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
                  sh 'docker build -t raushan8586/node-app-3.0 .'
                }
            }
        }


        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devopsuserdocker', variable: 'devopsuserdocker')]) {
                    sh 'docker login -u raushan8586 -p ${devopsuserdocker}'
            }
            sh 'docker push raushan8586/node-app-3.0'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name eksdemo1 --region ap-south-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }

  }
}
