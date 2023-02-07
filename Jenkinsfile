pipeline {
    agent {label 'nodejenkins'}
    stages {
        stage('Clone repository') { 
            steps {
              git (
                branch: 'main',
                url: 'https://github.com/gabriela-gnsales/ada-ci' 
              )
            }
        }
        stage('Build dev'){
            when {
               not {
                  branch "main"
               }
            }
            steps { 
                script{
                 image = docker.build("gabrielagns/v1:develop")
                 
                }
            }
        }
        stage('Build') {
            when {
                branch "main"
            } 
            steps { 
                script{
                 image = docker.build("gabrielagns/v1:main")
                }
            }
        }
        stage('Push') {
            steps {
                script{
                       docker.withRegistry('', 'gabrielagns-dockerhub') {
                       image.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                 script {
                    docker.image('gabrielagns/v1:main').withRun('-p 3000:3000 -d') {c ->
                        sleep 300
                    }
                }
            }
        }
    }
}
