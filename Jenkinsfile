def runServer() {
  sh 'docker run --name fordastore-cors --network fordastore --network-alias fordastore-cors -p 80:80 -d splitscale/fordastore-cors:latest'
}

pipeline {
    agent any

    stages {

        stage('pull') {
          steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/splitscale/nginx-gateway.git']]])
          }
        }

        stage('build docker image') {
          steps {
            sh 'docker build -t splitscale/fordastore-cors:latest .'
          }
        }

        stage('deploy') {
          steps {
            script {
              try {
                runServer()
              } catch (Exception e) {
                sh 'docker stop fordastore-cors'
                sh 'docker rm fordastore-cors'
                runServer()
              }
            }
          }
        }
    }
}
