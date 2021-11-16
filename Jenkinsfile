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
    - name: dockersock
      mountPath: /var/run/docker.sock   
  - name: dockerkubectl
    image: abimasantos/containerkubectl:v4
    command: ['cat']
    tty: true         
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) 
  {
    node(POD_LABEL) {
      checkout scm
      agent any
    
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

        stage('Deploy k8s') {
              
          agent {
            kubernetes {
              cloud 'kubernetes'
            }
          }
            
            steps {
              kubernetesDeploy(config: 'k8s/mongodb/deployment.yaml', kubeconfigId: 'kube')
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
