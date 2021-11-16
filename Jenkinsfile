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
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: dockers
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock      
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) 
  {

  environment {
      registry = "abimaesantos/pedelogo-catalogo"
      registryCredential = 'dockerhub'
   }

   
    node(POD_LABEL) {
    
        stage('git clone') {
          container('docker') {
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
           }
        }
        
        stage('docker build') {
          container('docker') {
            dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
            '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
            }
          }     
        
        
        stage('Docker push')
              environment {
               registryCredential = 'dockerhub'
           }
        container('docker'){
          docker.withRegistry( '', registryCredential ) {
          dockerapp.push()
          dockerapp.push('latest')
        }
    
    }
  }
}

// backup
//    def image = "jenkins/jnlp-slave"
//    node(POD_LABEL) {
//      stage('Build Docker image') {
//        git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
//        container('docker') {
//          sh "docker build -t ${image} ."
//        }
//      }
//    }



//  node(POD_LABEL) {
//        stage('git clone') {
//          container('docker-container') {
//            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
//              
//        stage('docker build') {
//          container('docker-container') {
//            dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
//            '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
//            }
//          }
//         
//        stage('docker push') {
//          container('docker-container') {
//            docker.withRegistry( '', registryCredential ) { 
//            dockerImage.push('latest') 
//            dockerImage.push("${env.BUILD_ID}")
//                  }
//          }
//          }
//        }
//      }
//  }





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
