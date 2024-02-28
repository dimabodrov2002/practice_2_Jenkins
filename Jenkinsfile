pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-8-slim'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Publish Artifact') {
            steps {
                archiveArtifacts(artifacts: '**/target/*.jar', fingerprint: true)
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
