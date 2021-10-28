pipeline { 
    agent any

environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }

  stages {
    stage('Get source'){
        steps{
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
    }

    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }

    stage('Build') {
      steps {
        sh 'docker build -t abimasantos/pedelogo-catalogo:latest -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
      }
    }

    stage('Push') {
      steps {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
        sh 'docker push abimasantos/pedelogo-catalogo:latest'
      }
    }
  }
}   