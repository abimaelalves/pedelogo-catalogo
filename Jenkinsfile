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
        '''
    }
  }
  stages {
    stage('Run maven') {
      steps {
        container('docker') {
          sh 'ls -l'
          sh 'echo test'
        }
//        container('node') {
//          sh 'npm version'
//          sh 'cat hello.txt'
//          sh 'ls -last'
//        }
      }
    }
  }
}