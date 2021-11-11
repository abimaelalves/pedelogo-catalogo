podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: docker-container-git
        image: alpine/git:latest
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

      - name: docker-container
        image: docker:19.03.8
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
    stage('git clone') {
      container('docker-container-git') {
        stage('git clone') {
          sh 'git clone https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
      }
    }


    stage('Building our image') {
      container('docker-container') {
        stage('Building our image') {
          sh 'docker build -t abimasantos/pedelogo-catalogo:v1 -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
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
