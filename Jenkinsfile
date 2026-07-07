pipeline {
    agent any

    environment {
        DOCKER_API_VERSION = '1.40'
    }

    stages {
        stage('Instalar dependencias') {
            agent {
                // Cambiado de 'docker' a 'dockerContainer' según los requisitos de tu Jenkins
                dockerContainer { image 'node:22-alpine' }
            }
            steps {
                sh 'npm install --no-audit --no-fund --update-notifier=false'
            }
        }

        stage('Ejecutar tests') {
            agent {
                dockerContainer { image 'node:22-alpine' }
            }
            steps {
                sh 'npm test'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}