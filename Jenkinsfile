pipeline {
    environment {
        registry = "ssarask/tp1"
        registryCredential = '7a22c70f-dd06-42b7-bfac-a122e274bb6d'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', 
                    credentialsId: '5cdda9c8-f00c-43ad-9611-e7f9dc96cb9a', 
                    url: 'https://github.com/SaraSekkoute/TP-VCDEVOPS'
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Testing Docker Image') {
            steps {
                script {
                    echo "Running tests on Docker image..."
                    // Remplacez par des tests réels si nécessaire
                    echo "Tests passed"
                }
            }
        }
        stage('Publishing Docker Image') {
            steps {
                script {
                    echo "Publishing Docker image to Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push() // Push avec le numéro de build
                        dockerImage.push('latest') // Push avec le tag 'latest'
                    }
                }
            }
        }
        stage('Deploying Docker Image') {
            steps {
                script {
                    echo "Deploying Docker image..."
                    // Commande pour lancer le conteneur Docker sur Windows
                    bat "docker run -d -p 8089:80 --name app-${BUILD_NUMBER} ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
    post {
        always {
            script {
                echo "Cleaning up workspace..."
                cleanWs() // Nettoie les fichiers temporaires du workspace Jenkins
            }
        }
        failure {
            script {
                echo "Pipeline failed. Check logs for more details."
            }
        }
        success {
            script {
                echo "Pipeline completed successfully!"
            }
        }
    }
}