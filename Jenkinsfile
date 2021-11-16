podTemplate(yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    env:
    - name: REGISTRY
      value: registry.hub.docker.com
    volumeMounts:
  - name: kubectl-container
    tty: true
    image: gcr.io/cloud-builders/kubectl
    command: ['cat']
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "500Mi"
        cpu: "1000m"
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) 
  {
    node(POD_LABEL) {
      checkout scm
    
        stage('Git Clone') {
          container('docker') {
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
           }
        }
        
        stage('Docker Build') {
          container('docker') {
            dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
            '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
            dockerapp = docker.build("abimasantos/pedelogo-catalogo:latest",
            '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
            }
          }   

        stage('Docker Push'){
          container('docker'){
            withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
              sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}'
              sh "docker push abimasantos/pedelogo-catalogo:${env.BUILD_ID}"
              sh 'docker push abimasantos/pedelogo-catalogo:latest'
            }
          }
        }

//     stage('Deploy K8S') {
//       steps {
//         echo "Deploy k8s"
//           container('kubectl-container') {
//             withKubeConfig([credentialsId: 'kube', serverUrl: 'http://172.18.0.2:32000']) {
//               sh """
//               kubectl apply -f k8s/mongodb/deployment.yaml
//               """  
//              }
//            }
//          }
//
//      }
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
