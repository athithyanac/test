pipeline {
    agent any

    options{
        timestamps()
        buildDiscarder(logRotator(numToKeepStr:'1'))
    }

    environment {
        imgName = "athithyanac/testapp"
        dockerTag = "v1"
    }

    stages {
        stage('Precheck') {
            parallel {
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
            }
        }
 
	    stage('Docker build') {
            when {
            expression {     
                return env.GIT_BRANCH == "origin/main"   
                       }
            }
            steps {
                sh "docker build -t ${imgName}:${env.BUILD_NUMBER} -t ${imgName}:latest ."
            }
        }
        stage('Docker Image Verify') {
            steps {
                sh 'docker image ls'
            }
        }
        stage('Docker Clean up') {
            steps {
                sh 'docker rm -f $(docker ps -aq) 2> /dev/null || true'
            }            
        }
        stage('Docker-Deploy') {
            input
            {
                message "Do you want to proceed for deployment ?"
            }
            steps {
                sh "docker run ${imgName}:${env.BUILD_NUMBER} ls -l"
            }            
        }
        stage('Docker-Images-CleanUp') {
            steps {        
                sh 'docker image prune -af'  
            }
        }
    }
    post {
        always {
            sh 'docker images'
        }
    }
}

