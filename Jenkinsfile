pipeline {
  environment {
    registry = "abimasantos/pedelogo-catalogo"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }   
    
    agent any

  stages {
    stage('Get source'){
        steps{
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
    }


    stage('Build') {
      steps {
        sh 'docker build -t abimasantos/pedelogo-catalogo:latest -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
      }
    }
    
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
  }
}
}