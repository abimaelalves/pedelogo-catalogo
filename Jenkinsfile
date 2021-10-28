pipeline {
    agent any

    ${env.BUILD_ID}s {
        
        ${env.BUILD_ID}('Get source'){
            steps{
                git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
            }
        }

        ${env.BUILD_ID}('Build and Push ${env.BUILD_ID} image') {
                when {
                    branch 'main'
                }
                steps {
                    script {
                    docker.withRegistry('') {
                        sh "docker build --build-arg -t abimasantos/pedelogo-catalogo:${env.BUILD_ID},
                            ' -f ./src/PedeLogo.Catalogo.Api/Dockerfile .' "
                        docker.image('somos99:${env.BUILD_ID}').push()
                    }
                    }
                }
                }
    }
}       
//
//        ${env.BUILD_ID}('Docker Build'){
//            steps {
//                script {
//                    dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
//                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
//                }
//            }
//        }
//
//        ${env.BUILD_ID}('Docker Push Image'){
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
