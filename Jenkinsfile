pipeline {
    agent any

    stages {
      
      stage('Get Source') {
        steps{
          git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
      }

      stage ('Docker Build AND push') {
        steps {
          script {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
            dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
              '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .').push('latest')
          }
        }
      }
      
        
//      stage ('Docker Push Image'){
//        steps {
//          script {
//            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
//            dockerapp.push('latest')
//            dockerapp.push("${env.BUILD_ID}")
//
//          }
//        }
//      }


    }
}



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
