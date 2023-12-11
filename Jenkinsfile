pipeline {
    agent any
    
    tools {
        maven 'maven'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/amine17589/javulna.git'
            }
        }
        
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn install'
            }
        }

        stage('docker build') {
            steps {
                sh 'docker build -t javulna-0.1 .'
            }
        }

        stage('Run SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('docker run container') {
            steps {
                sh 'docker stop app || true'
                sh 'docker rm app || true'
                sh 'docker run --name app -it -d -p 9001:8080 javulna-0.1'
            }
        }
    }
}
