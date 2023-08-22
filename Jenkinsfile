pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'Maven3'
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
                sh 'docker build . -t prashantl/shopping-cart:latest -f docker/Dockerfile'
            }
        }
        
        stage('Build And Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'Docker-Hub-Pwd', variable: 'DockerHub')]) {

                    sh 'docker login --username prashantl --password ${DockerHub}'
                    sh 'docker tag shopping-cart:latest prashantl/shoppig-cart:latest'
                    sh 'docker push prashantl/shopping-cart:latest'
                }
            }
        }
    }
}
