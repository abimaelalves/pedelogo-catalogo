podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: docker-container
        image: abimasantos/containerkubectl:v2
        command:
        args:
''') {
  node(POD_LABEL) {
    stage('test') {
      container('docker-container') {
        stage('test') {
          sh 'ls -l'
        }
      }
    }

  }
}

//podTemplate(yaml: '''
//    apiVersion: v1
//    kind: Pod
//    spec:
//      containers:
//      - name: docker-container
//        image: abimasantos/containerkubectl:v2
//        command: ['cat']
//        tty: true
//        resources:
//          requests:
//            memory: "64Mi"
//            cpu: "250m"
//          limits:
//            memory: "500Mi"
//            cpu: "1000m"
//        volumeMounts:
//        - mountPath: /var/run
//          name: docker-sock      
//      volumes:
//      - name: docker-sock                             
//        hostPath: 
//            path: /var/run
//''') {
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
//}




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
