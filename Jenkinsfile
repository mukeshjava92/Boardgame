pipeline {
    agent any
    tools {
        jdk 'jdk-17'
        maven 'maven3'
    }

    stages {
        stage('Git checkout') {
            steps {
                sh '''
                git branch:'main',credentialsId:'github',url:'https://github.com/mukeshjava92/Boardgame.git'
                '''
            }
        }
        stage('Compile the Source code') {
            steps {
                sh " mvn compile"
            }
        }
        stage('Test') {
            steps {
                sh " mvn test"
            }
        }
         stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-output.html ."
            }
        }
    }
}
