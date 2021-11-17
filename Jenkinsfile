pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:1.11
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

        container('docker') {
            sh 'docker build -t abimasantos/pedelogo-catalogo:latest -f ./src/PedeLogo.Catalogo.Api/Dockerfile'
        }
      }
    }
  }
}