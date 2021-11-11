podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: jenkins-slave
        image: jenkinsci/jnlp-slave
        command:
        - sleep
        args:
        - 99d
''') {
  node(POD_LABEL) {
    stage('Git Clone') {
      git 'https://github.com/abimaelalves/pedelogo-catalogo.git'
      container('jenkins-slave') {
        stage('Clone') {
          sh 'ls -l'
        }
      }
    }

    
//    stage('Docker Build') {
//      container('jenkins-slave') {
//        stage('Docker build') {
//          steps {
//            script {
//              dockerImage = docker.build registry + ":${env.BUILD_ID}",
//                  '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
//            }
//          }
//        }
//      }
//    }

//    stage('Docker push') {
//      container('jenkins-slave') {
//        stage('teste2') {
//          sh 'echo teste'
//        }
//      }
//    }    
//
//    stage('Docker cleaner') {
//      git 'https://github.com/jenkinsci/kubernetes-plugin.git'
//      container('jenkins-slave') {
//        stage('teste2') {
//          sh 'echo teste'
//        }
//      }
//    }  

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
