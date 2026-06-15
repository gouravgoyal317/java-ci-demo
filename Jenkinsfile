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
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhubCred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin' 
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker tag springboot-demo:latest sourabgoyal82/springboot-demo:latest'
                sh 'docker push sourabgoyal82/springboot-demo:latest'
            }
        }
        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f springboot || true'
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
