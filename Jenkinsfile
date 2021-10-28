pipeline {
   environment {
      registry = "abimasantos/pedelogo-catalogo"
      registryCredential = 'dockerhub'
      dockerImage = ''
   }
   agent any
   stages {
      stage('Cloning Git') {
         steps {
            git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
         }
      }
      stage('Building image') {
         steps {
            script {
               dockerImage = docker.build registry + ' -f ./src/PedeLogo.Catalogo.Api/Dockerfile' ":latest"
            }
         }
      }
      stage('Deploy Image') {
         steps {
            script {
               docker.withRegistry('', registryCredential) {
                  dockerImage.push()
               }
            }
         }
      }
      stage('Remove Unused docker image') {
         steps {
            sh "docker rmi $registry:latest"
         }
      }
   }
}