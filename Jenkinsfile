pipeline {
    agent any

    environment {
        imgName = "athithyanac/testapp"
    }

    stages {
        stage('Docker Verify') {
            steps {
                sh 'docker --version'
            }
        }
        stage('Git Verify') {
            steps {
                sh 'git --version'
            }
        }
	    stage('Docker build') {
            steps {
                sh "docker build -t ${imgName}:v1 -t ${imgName}:latest .'
            }
        }
        stage('Docker Image Verify') {
            steps {
                sh 'docker image ls'
            }
        }
	
    }
}

