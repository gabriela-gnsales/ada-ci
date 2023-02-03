pipeline {
    agent {label 'nodeprojeto'}
    environment {
	DOCKERHUB_CREDENTIALS = credentials('gabrielagns-dockerhub')
    }
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
                       docker.withRegistry('', 'docker') {
                       image.push()
                    }
                }
            }
        }
    }
}
