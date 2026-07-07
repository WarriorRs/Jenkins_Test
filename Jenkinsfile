pipeline {
    agent any

    tools {
        // Usamos la versión estable que acabas de configurar en la interfaz web
        nodejs "Node20"
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                // Instalación limpia evitando cuelgues de red en la universidad
                sh 'npm install --no-audit --no-fund --update-notifier=false'
            }
        }

        stage('Ejecutar tests') {
            steps {
                // Ejecuta los tests nativos definidos en tu package.json
                sh 'npm test'
            }
        }

        stage('Arrancar Aplicación') {
            steps {
                echo '¡Dependencias instaladas y pruebas superadas con éxito!'
                // Si la tarea solo pide probar la app en integración continua, con esto basta.
                // Si necesitas dejarla corriendo en segundo plano de manera nativa:
                // sh 'npm start &'
            }
        }
    }
}