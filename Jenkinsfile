pipeline {
    agent any

    environment {
        DOCKER_API_VERSION = '1.40'
    } // <-- Aquí faltaba cerrar esta llave

    tools {
        nodejs "Node25"
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                // Añadidas banderas para evitar que NPM se cuelgue en Jenkins
                sh 'npm install --no-audit --no-fund --update-notifier=false'
            }
        }

        stage('Ejecutar tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Construir Imagen Docker') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
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
    } // <-- Cierra stages
} // <-- Cierra pipeline