// pipeline {
//     agent any

//     stages {

//         stage('Check Docker') {
//             steps {
//                 bat 'docker --version'
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 bat 'docker build -t myapp .'
//             }
//         }

//         stage('Run Docker Container') {
//             steps {
//                 bat 'docker run --rm myapp'
//             }
//         }
//     }
// }

// pipeline {
//     agent {
//         docker {
//             image 'python:3.11-slim'
//         }
//     }

//     stages {
//         stage('Check Python Version') {
//             steps {
//                 sh 'python --version'
//             }
//         }

//         stage('Check Pip Version') {
//             steps {
//                 sh 'pip --version'
//             }
//         }
//     }
// }

pipeline {

    agent any

    environment {
        IMAGE_NAME = "kamakshiagg/flask-jenkins"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Code Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat 'docker push %IMAGE_NAME%:%IMAGE_TAG%'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                //docker rm -f flask-app 2>nul

                docker run -d --name flask-app -p 5000:5000 %IMAGE_NAME%:%IMAGE_TAG%

                docker ps
                '''
            }
        }
    }

    post {

        success {
            echo 'Docker image built, pushed, and deployed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            bat 'docker images'
        }
    }
}