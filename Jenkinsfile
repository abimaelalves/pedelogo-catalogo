pipeline {
    agent any

    stages {
        
        stage('Get source'){
            steps{
                git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
            }
        }

        stage('Build and Push stage image') {
                }
                steps {
                    script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        sh "docker build --build-arg -t abimasantos/pedelogo-catalogo:${env.BUILD_ID} -f ./src/PedeLogo.Catalogo.Api/Dockerfile . "
                        docker.image('abimasantos/pedelogo-catalogo').push()
                    }
                    }
                }
                } 
    }
}       
//
//        stage('Docker Build'){
//            steps {
//                script {
//                    dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
//                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
//                }
//            }
//        }
//
//        stage('Docker Push Image'){
//            steps {
//                script{
//                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
//                    dockerapp.push('latest')
//                    dockerapp.push("${env.BUILD_ID}")
//                }
//            }
//
//        }
//    }
//}
