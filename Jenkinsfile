PROJECT_NAME = "api-produto"

pipeline { 
  agent { 
    kubernetes {
      label "worker-${PROJECT_NAME}-${UUID.randomUUID().toString()}"
      yamlFile './JenkinsContainers.yml'
    }
  }
  
  options {
    parallelsAlwaysFailFast()
  }

 
  environment {
      registry = "abimasantos/pedelogo-catalogo" 
      registryCredential = 'dockerhub' 
      dockerImage = '' 
  }

/*Pipeline para o ambiente de QA*/
  stages { 
      stage('Cloning our Git') { 
          steps { 
              git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
          }
      } 

      stage('Building our image') { 
          steps { 
              script { 
                  dockerImage = docker.build registry + ":${env.BUILD_ID}",
                  '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
              }
          } 
      }

      stage('Deploy our image') { 
          steps { 
              script { 
                  docker.withRegistry( '', registryCredential ) { 
                  dockerImage.push('latest') 
                  dockerImage.push("${env.BUILD_ID}")
                  }
              } 
          }
      } 

      stage('Cleaning up') { 
          steps { 
            sh "docker rmi $registry:${env.BUILD_ID}" 
            sh "docker rmi $registry:latest" 
          }
      } 

     stage('Deploy K8S') {
       when {
         branch 'qa'
       }
       steps {
         echo "Deploy k8s"
           container('kubectl-container') {
             withKubeConfig([credentialsId: 'kube', serverUrl: 'https://192.168.0.8:6443']) {
               sh """
               kubectl apply -f k8s/mongodb/deployment.yaml
               """  
              }
            }
          }

     }
  }
  
}
