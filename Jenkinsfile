pipeline {
    agent any
    stages {
        stage('Supprimer le workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout SCM') {
            steps {
                git branch: 'main', credentialsId: 'id-user-github', url: 'https://github.com/ksdeve/projet-dev01.git'
            }
        }
         stage('Build image docker') {
            steps {
                script{
                    sh 'docker build -t myimage_nginx .'
                    sh 'docker tag myimage_nginx kevins:myimage_nginx'
                }
            }
        }
         stage('Deploiement application') {
            steps {
                script{
                    sh 'docker stop monapp'                  
                    sh 'docker rm monapp'                  
                    sh 'docker run -d --name monapp --hostname monapp -p 8099:80 myimage_nginx'
                    sh 'docker exec monapp "ifconfig"'
                }
            }
        }
    }
}
