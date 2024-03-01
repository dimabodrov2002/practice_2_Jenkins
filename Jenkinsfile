pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Проверяем конфигурацию репозитория Git
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.image('maven:3.6.3-jdk-8-slim').inside {
                        sh 'mvn clean package'
                    }
                }
            }
        }
        stage('Publish Artifact') {
            steps {
                script {
                    archiveArtifacts(artifacts: '**/target/*.jar', fingerprint: true)
                }
            }
        }
    }
    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
