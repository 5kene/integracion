pipeline {
    agent any

    tools {
        nodejs "Node25"        // 👈 lo dejamos, no se usa directamente
        dockerTool "Dockertool"
    }

    stages {

        stage('Instalar dependencias') {
            steps {
                sh '''
                    docker run --rm \
                      -v "$PWD:/app" \
                      -w /app \
                      node:25 \
                      npm install
                '''
            }
        }

        stage('Ejecutar tests') {
            steps {
                sh '''
                    docker run --rm \
                      -v "$PWD:/app" \
                      -w /app \
                      node:25 \
                      npm test
                '''
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            steps {
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d \
                      --name hola-mundo-node \
                      -p 3000:3000 \
                      hola-mundo-node:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline OK con Node 25'
        }
        failure {
            echo '❌ Pipeline falló'
        }
    }
}
