podTemplate(yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: docker-sock                           # montando o volume no container
      mountPath: /var/run
  volumes:
  - name: docker-sock                             
    hostPath: 
        path: /var/run       
"""
  ) {

  def image = "jenkins/jnlp-slave"
  node(POD_LABEL) {
    stage('Build Docker image') {
      git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
      container('docker') {
        sh "docker build -t ${image} ."
      }
    }
  }
}
//
//pipeline { 
//  environment { 
//      registry = "abimasantos/pedelogo-catalogo" 
//      registryCredential = 'dockerhub' 
//      dockerImage = '' 
//  }
//  agent any 
//  stages { 
//      stage('Cloning our Git') { 
//          steps { 
//              git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
//          }
//      } 
//
//      stage('Building our image') { 
//          steps { 
//              script { 
//                  dockerImage = docker.build registry + ":${env.BUILD_ID}",
//                  '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
//              }
//          } 
//      }
//
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
//       agent {
//         kubernetes {
//             cloud 'kubernetes'
//         }
//       }
//         steps {
//             kubernetesDeploy(configs: 'k8s/mongodb/deployment.yaml', kubeConfig: kubeconfig)
//         }
//       }

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
//  }
//}

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
