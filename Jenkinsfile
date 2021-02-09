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
                sh 'trivy image --format json --output results.json hello-brunch'
            }
            post{
                always{
                    recordIssues(tools: [trivy(pattern: 'results.json')])
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

