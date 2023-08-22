pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'maven3'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Devops', url: 'https://github.com/Prashant2801/Shopping-Cart.git']])
                
            }
        }
        stage('Code Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Build Application') {
            steps {
                sh 'mvn install -DskipTests' 
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t prashantl/shopping-cart:latest .'
            }
        }
        
        stage('Build And Push Docker Image') {
            steps {
                withCredentials([credentialsId: '3c5f335d-db92-4417-825c-b90b8470ebc9', variable: 'dockerHubPwd')] {
                    sh 'docker login --username prashantl --password ${dockerHubPwd}'
                    sh 'docker build -t prashantl/shopping-cart:latest'
                    sh 'docker push prashant/shopping-cart:latest'
                }
            }
        }
    }
}
