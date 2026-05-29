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

pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
        }
    }
    stages {
        stage('Check Python') {
            steps {
                bat 'python --version'
            }
        }
        stage('Check pip') {
            steps {
                bat 'pip --version'
            }
        }
    }
}
