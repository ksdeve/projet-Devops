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
               git branch: 'main', credentialsId: 'ksdeve-github-id', url: 'https://github.com/ksdeve/projet-Devops.git'
            }
        }
         stage('Build image docker') {
            steps {
                script{
                    sh 'docker build -t myapp-image .'
                    sh 'docker tag myapp-image kevins:myapp-image'
                }
            }
        }
         stage('Deploiement application') {
            steps {
                script{
          // Nettoyage des anciens conteneurs (s'ils existent)
            sh 'docker ps -a --format "{{.Names}}" | grep -w myapp && docker stop myapp || true'
            sh 'docker ps -a --format "{{.Names}}" | grep -w myapp && docker rm myapp || true'

            // Déployer le nouveau conteneur
            sh 'docker run -d --name myapp --hostname myapp -p 8088:80 myapp-image'

            // Vérification réseau
            sh 'docker exec myapp ifconfig'
                }
            }
        }
    }
}
