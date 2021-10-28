pipeline { 
  environment { 
      registry = "abimasantos/pedelogo-catalogo" 
      registryCredential = 'dockerhub' 
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
                    dockerImage = docker.build registry + ' -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'":$BUILD_NUMBER" 
                }
            } 
        }


      } 
      
  }

