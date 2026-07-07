pipeline {
    agent any

    environment {
        DOCKER_API_VERSION = '1.40'
    }

    stages {
        // Ejecutamos la instalación y los tests directamente usando el comando docker build original
        // Esto evita el "Launching new docker node" que se quedó trabado
        stage('Instalar dependencias y Construir Imagen') {
            steps {
                // Construye la imagen usando tu Dockerfile (el cual ya tiene el "npm install" adentro)
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