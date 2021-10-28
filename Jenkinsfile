pipeline { 

    environment {
      registry = "abimaesantos/pedelogo-catalogo"
      registryCredential = 'dockerhub'
   }
    
    agent any


  stages {
    stage('Git clone'){
        steps{
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
    }

//    stage('Build docker image') {
//      steps {
//        sh 'docker build -t abimasantos/pedelogo-catalogo:latest -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
//      }
//    }

    stage('Building image') {
         steps {
            script {
               dockerImage = docker.build registry +'latest' ' -f ./src/PedeLogo.Catalogo.Api/Dockerfile'
            }
         }
      }
    
    stage('Push docker Image') {
    steps {
    script {
        docker.withRegistry('', registryCredential) {
            sh 'docker push abimasantos/pedelogo-catalogo:latest'
        }
    }
    }
    }
  }
}
