pipeline { 
  environment { 
      registry = "abimasantos/pedelogo-catalogo" 
      registryCredential = 'dockerhub' 
      dockerImage = '' 
  }
  agent any 
  
  stages { 
      stage('Cloning our Git') { 
          steps { 
              git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main' 
          }
      } 

      stage('Building our image') { 
          steps { 
              script { 
                  dockerImage = docker.build registry + ":$BUILD_NUMBER" 
              }
          } 
      }
        
//      stage('Deploy our image') { 
//          steps { 
//              script { 
//                  docker.withRegistry( '' '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .'":$BUILD_NUMBER" registryCredential ) { 
//                      dockerImage.push() 
//                  }
//              } 
//          }
      } 
      stage('Cleaning up') { 
          steps { 
              sh "docker rmi $registry:$BUILD_NUMBER" 
          }
      } 
  }
}


