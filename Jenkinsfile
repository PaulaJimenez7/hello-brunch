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
                sh 'trivy filesystem --format json --output results.json .'
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

