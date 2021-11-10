pipeline { 
  environment { 
      registry = "abimasantos/pedelogo-catalogo" 
      registryCredential = 'dockerhub' 
      dockerImage = '' 
  }

    agent {
    kubernetes {
      label 'master'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kubectl-container
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: '/var/run'
      name: docker-sock
  volumes:
  - name: docker-sock
    hostPath: 
        path: /var/run
"""
    }
  }
  
    stages { 
//        stage('Cleaning up') { 
//          steps { 
//            sh "apt update && apt install curl" 
//            sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/linux/amd64/kubectl" 
//            sh "chmod +x ./kubectl" 
//            sh "mv kubectl /usr/local/bin"
//            sh "kubectl get pod" 
//          }
//      } 
//    }
//}

      stage('Cloning our Git') { 
          steps { 
              git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
          }
      } 

      stage('Building our image') { 
          steps { 
              script { 
                  dockerImage = docker.build registry + ":${env.BUILD_ID}",
                  '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
              }
          } 
      }
      
      stage('Deploy our image') { 
          steps { 
              script { 
                  docker.withRegistry( '', registryCredential ) { 
                  dockerImage.push('latest') 
                  dockerImage.push("${env.BUILD_ID}")
                  }
              } 
          }
      } 

      stage('Cleaning up') { 
          steps { 
            sh "docker rmi $registry:${env.BUILD_ID}" 
            sh "docker rmi $registry:latest" 
          }
      } 

     stage('Deploy K8s') {
          steps { 
            sh "apt update && apt install curl" 
            sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/linux/amd64/kubectl" 
            sh "chmod +x ./kubectl" 
            sh "mv kubectl /usr/local/bin"
            sh "kubectl apply -f k8s/mongodb/deployment.yaml" 
          }
        }
    }
         
}

//      stage('Deploy our image') { 
//          steps { 
//              script { 
//                  docker.withRegistry( '', registryCredential ) { 
//                  dockerImage.push('latest') 
//                  dockerImage.push("${env.BUILD_ID}")
//                  }
//              } 
//          }
//      } 
//
//      stage('Cleaning up') { 
//          steps { 
//            sh "docker rmi $registry:${env.BUILD_ID}" 
//            sh "docker rmi $registry:latest" 
//          }
//      } 
//
//     stage('Deploy K8s') {
//         steps {
//            container('kubectl-container'){
//              withKubeConfig([credentialsId: 'CONFIGMANEID', serverUrl: K8SURL]) {
//                sh """
//                kubectl apply -f k8s/mongodb/deployment.yaml
//                kubectl -n NAMESPACE rollout restart deployment/DEPLOYMENTNAME
//                """
//              }
//            }
//         }
//       }
//    }
//}    


//     stage('Deploy Kubernetes') {
//       steps {
//         echo "Deploy k8s"
//            container('kubectl-container'){
//               withCredentials([file(credentialsId: 'kube', variable: 'KUBECONFIG')]) {
//                sh """
//                kubectl apply -f k8s/mongodb/deployment.yaml
//                """
//              }
//            }
//       }
//     }

//      stage('Deploy Kubernetes'){
//          agent {
//            kubernetes {
//                cloud 'kubernetes'
//            }
//          }
//            steps{
//                kubernetesDeploy(configs: 'k8s/mongodb/deployment.yaml', kubeconfigId: 'kubeconfig')
//            }                    
//      }

/////////// fecha pipeline ////////
///  }
///}

// pipeline abaixo funcionou, deixando como opção de uso
//pipeline { 
//
//    environment {
//      registry = "abimaesantos/pedelogo-catalogo"
//      registryCredential = 'dockerhub'
//   }
//    
//    agent any
//
//
//  stages {
//    stage('Git clone'){
//        steps{
//            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
//        }
//    }
//
//    stage('Build docker image') {
//      steps {
//        sh 'docker build -t abimasantos/pedelogo-catalogo:v1 -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
//      }
//    }
//    
//    stage('Push docker Image') {
//    steps {
//    script {
//        docker.withRegistry('', registryCredential) {
//            sh 'docker push abimasantos/pedelogo-catalogo:v1'
//        }
//    }
//    }
//    }
//  }
//}
