pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging' 
            }
        }
        stage('Deploy to Prod') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve PROD deployment?'
                }
                build job: 'deploy-to-prod'               
            }
            post {
                success {
                    echo 'Code deloyed to prod'
                }
                failure {
                    echo 'Deployment failed'
                }
            }
        }
    }
}