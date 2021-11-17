pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: abimasantos/containerkubectl:v4
            command:
            - cat
            tty: true
            env:
            - name: REGISTRY
              value: registry.hub.docker.com
            volumeMounts:
              - name: dockersock
                mountPath: /var/run/docker.sock 
          volumes:
          - name: dockersock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {
    stage('Git clone') {
      steps {
        container('docker') {
          git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }

    stage('Docker Build') {
      steps {
        container('docker') {
          sh "docker build -t abimasantos/pedelogo-catalogo:${env.BUILD_ID} -f ./src/PedeLogo.Catalogo.Api/Dockerfile ."
        }
      }
    }
//
//        container('docker') {
//          sh "docker build -t abimasantos/pedelogo-catalogo:${env.BUILD_ID} -f ./src/PedeLogo.Catalogo.Api/Dockerfile ."
//        }
//
//        container('docker'){
//          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
//            sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}'
//            sh "docker push abimasantos/pedelogo-catalogo:${env.BUILD_ID}"
//            }
//          }
//        
//        container('docker') {
//          sh 'kubectl'
//        }
      }
    }
  }
}