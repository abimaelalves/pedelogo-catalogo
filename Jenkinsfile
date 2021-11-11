podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: docker-container
        image: abimasantos/containerkubectl:v1
        command: ['cat']
        tty: true
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "500Mi"
            cpu: "1000m"
        volumeMounts:
        - mountPath: /var/run
          name: docker-sock
      - name: docker-container2
        image: abimasantos/containerkubectl:v1
        command: ['cat']
        tty: true
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "500Mi"
            cpu: "1000m"
        volumeMounts:
        - mountPath: /var/run
          name: docker-sock          
      volumes:
      - name: docker-sock                             
        hostPath: 
            path: /var/run
''') {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      container('docker-container') {
        stage('git clone') {
          sh 'docker ps'
          sh 'ls -l'
          sh 'pwd'
          sh 'df -h'
        }
      }
    }

    stage('clone') {
      container('docker-container2') {
        stage('git clone2') {
          sh 'git clone https://github.com/abimaelalves/pedelogo-catalogo.git'
          sh 'ls -l'
          sh 'cd pedelogo-catalogo'
          sh 'ls -l'
        }
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
