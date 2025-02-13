pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/Harsha2289/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
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
                  sh 'docker build -t harshapatil642/node-app-3.0 .'
                }
            }
        }


        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devopsuserdocker', variable: 'devopsuserdocker')]) {
                    sh 'docker login -u harshapatil642 -p ${devopsuserdocker}'
            }
            sh 'docker push harshapatil642/node-app-3.0'
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
