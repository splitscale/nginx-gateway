
pipeline {
    agent any

    stages {
        stage('pull') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/splitscale/fordastore-cors.git']]])
      }
        }

        stage('build docker image') {
      steps {
        sh 'docker build -t kasutu/fordastore-cors:latest .'
      }
        }

        stage('deploy') {
          steps {
            script {
              withCredentials([usernamePassword(credentialsId: 'docker-pwd', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                sh "docker login -u ${USERNAME} -p ${PASSWORD}"
              }

              sh "docker push kasutu/fordastore-cors:latest"
            }
          }
        }
    }
}
