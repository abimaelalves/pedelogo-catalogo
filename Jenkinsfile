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
    image: abimasantos/containerkubectl:v1
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
            container('kubectl-container'){
              withKubeConfig([credentialsId: 'CONFIGMANEID', serverUrl: 'https://192.168.0.8:6443']) {
                sh """
                kubectl get pod
                """
              }
            }
         }
       }
    
    }

}
//##############################

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
