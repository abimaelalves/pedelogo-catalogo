pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          name: pod-docker-container
        spec:
          containers:
          - name: docker-container
            image: docker:19.03.8
            command: ['cat']
            tty: true
            resources:
              requests:
                memory: "64Mi"
                cpu: "250m"
              limits:
                memory: "500Mi"
                cpu: "1000m"
            env:
            - name: REGISTRY
              value: registry.hub.docker.com
            volumeMounts:
            - name: docker-sock                        
              mountPath: /var/run
          volumes:
          - name: docker-sock                             
            hostPath: 
              path: /docker    
        '''
    }
  }
  stages {
    stage('Git clone') {
      steps {
        container('docker') {
          git url: 'https://github.com/abimaelalves/pedelogo-catalogo.git', branch: 'main'
        }
      }
    }
    stage('Docker Build') {
      steps {
        container('docker') {
          sh 'docker build -t abimasantos/pedelogo-catalogo:v1 -f ./src/PedeLogo.Catalogo.Api/Dockerfile .'
        }
      }
    }
    stage('Docker Push') {
      steps {
        container('docker'){
            withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
            sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}'
            sh "docker push abimasantos/pedelogo-catalogo:v1"
            }
          }
      }
    }

    stage('Deploy k8s') {
      agent {
        kubernetes {
          cloud 'kubernetes'          
        }
      }

      steps {
        kubernetesDeploy(configs: 'k8s/mongodb/deployment.yaml', kubeconfigId: 'kubeconfig')
      }

    }
  }
}