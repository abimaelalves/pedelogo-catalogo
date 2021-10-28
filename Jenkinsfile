pipeline {
    agent any

    stages {
        
        stage('Get source'){
            steps{
                git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
            }
        }

        stage('Docker Build'){
            steps {
                script {
                    dockerapp = docker.build("abimasantos/pedelogo-catalogo:${env.BUILD_ID}",
                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }

        stage('Docker Push Image'){
            steps {
                script{
                    withRegistry('https://registry.hub.docker.com', 'dockerhub')
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                }
            }

        }
    }
}
