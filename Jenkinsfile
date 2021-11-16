pipeline { 

  environment { 
      registry = "abimasantos/pedelogo-catalogo" 
      registryCredential = 'dockerhub' 
      dockerImage = '' 
  }

    agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
    }
  }
  
    stages { 
    container('docker') {
      stage('git clone') {    
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
           }
        }
    container('docker') {
      stage('docker build') {
          container('docker') {
            dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
            '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
            }
          }

      
      stage('Deploy our image') { 
          container('docker'){
          steps { 
              script { 
                  docker.withRegistry( '', registryCredential ) { 
                  dockerImage.push('latest') 
                  dockerImage.push("${env.BUILD_ID}")
                  }
              } 
          }
         } 
        }

      stage('Cleaning up') { 
          container('docker'){
          steps { 
            sh "docker rmi $registry:${env.BUILD_ID}" 
            sh "docker rmi $registry:latest" 
          }
      } 
      }
    
    }

}