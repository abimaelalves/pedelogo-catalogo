pipeline { 
    agent any

  stages {
    stage('Get source'){
        steps{
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
    }

    stage('Build') {
      steps {
        sh 'docker build -t abimasantos/pedelogo-catalogo:latest .'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push abimaelalves/pedelogo-catalogo:latest'
      }
    }
  }
}   