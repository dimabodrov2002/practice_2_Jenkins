pipeline {
    agent any
    environment {
        DOCKER_AVAILABLE = "false"
    }
    stages {
        stage('Check Docker') {
            steps {
                script {
                    if (isUnix()) {
                        // Проверка доступности Docker на системах UNIX
                        DOCKER_AVAILABLE = sh(script: 'which docker', returnStatus: true) == 0 ? "true" : "false"
                    } else {
                        // Проверка доступности Docker на других системах
                        DOCKER_AVAILABLE = bat(script: 'where docker', returnStatus: true) == 0 ? "true" : "false"
                    }
                }
            }
        }
        stage('Build') {
            when {
                expression { return env.DOCKER_AVAILABLE == "true" }
            }
            steps {
                script {
                    docker.image('maven:3.6.3-jdk-8-slim').inside {
                        sh 'mvn clean package'
                    }
                }
            }
        }
        stage('Publish Artifact') {
            when {
                expression { return env.DOCKER_AVAILABLE == "true" }
            }
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
