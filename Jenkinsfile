pipeline {
    agent { label 'sourab' }
    stages {
        stage('Clone') {
            steps {
                echo 'https://github.com/gouravgoyal317/java-ci-demo.git'
            }
        }
        stage('Build Jar') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker build') {
            steps {
                sh 'docker build -t springboot-demo:latest .'
            }
        }
        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f springboot-demo || true'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker run -d --name springboot -p 8082:8080 springboot-demo:latest'
            }
        }
        stage('Verify') {
            steps {
                sh 'docker ps'
            }
        }
    }
}
