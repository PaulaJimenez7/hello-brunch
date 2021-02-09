#!/usr/bin/env groovy
pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage("trivy"){
            steps{
                sh 'trivy filesystem -f json -o trivy-fs.json .'
                sh 'trivy image --format json --output trivy-image.json hello-brunch'
            }
            post{
                always{
                    recordIssues enabledForFailure: true, aggregatingResults:true, tool: trivy(pattern: 'trivy-*.json')
                }
            }
        }
        stage('deploy') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

