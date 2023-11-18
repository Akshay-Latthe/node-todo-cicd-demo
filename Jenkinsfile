pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/aws-account-project/node-todo-cicd-demo.git", branch: "main"
            }
        }
        stage("Build & Test") {
            steps {
                echo "Building Docker image"
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHub_PASSWORD', usernameVariable: 'dockerHub_USERNAME')]) {
                    sh "docker login -u ${env.dockerHub_USERNAME} -p ${env.dockerHub_PASSWORD}"
                    sh "docker tag node-app-test-new ${env.dockerHub_USERNAME}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHub_USERNAME}/node-app-test-new:latest"
                    echo "Image has been pushed to DockerHub"
                }
            }
        }
        stage("Deploy App") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
