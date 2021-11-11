podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: docker-container
        image: docker:19.03.8
        command:
        - cat
        tty: true
      - name: golang
        image: golang:1.16.5
        command:
        - sleep
        args:
        - 99d
''') {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      container('docker-container') {
        stage('git clone') {
          sh 'git clone https://github.com/jenkinsci/kubernetes-plugin.git'
          sh 'pwd'
          sh 'ls -l'
        }
      }
    }

//    stage('Get a Golang project') {
//      git url: 'https://github.com/hashicorp/terraform-provider-google.git', branch: 'main'
//      container('golang') {
//        stage('Build a Go project') {
//          sh '''
//            mkdir -p /go/src/github.com/hashicorp
//            ln -s `pwd` /go/src/github.com/hashicorp/terraform
//            cd /go/src/github.com/hashicorp/terraform && make
//          '''
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
