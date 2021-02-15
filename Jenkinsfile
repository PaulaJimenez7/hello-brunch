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
        stage("Publish"){
            steps{
                withDockerRegistry([credentialsId: "gitlab-registry", url: "http://10.250.13.1:5050"]) {
                    sh 'docker tag hello-brunch:latest 10.250.13.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                    sh 'docker push 10.250.13.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                    sh 'git tag BUILD-1.${BUILD_NUMBER}'
                    sh 'git push 10.250.13.1:8929/root/hello-brunch --tags'
                }
            }
        }
    }
}

