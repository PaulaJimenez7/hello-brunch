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
                    sh 'docker tag hello-brunch:latest 10.250.13.1:5050/root/hello-brunch:latest'
                    sh 'docker push 10.250.13.1:5050/root/hello-brunch:latest'
                    sshagent(credentials:['gitlab-ssh']){
                        sh 'git tag BUILD-1.${BUILD_NUMBER}'
                        sh 'git push --tags'
                    }                  
                }
            }
        }
        stage('Deploy'){
            steps{
                sshagent(credentials:['deploy-ssh']){
                    sh 'ssh -t -o "StrictHostKeyChecking no" deploy@10.250.13.1 "docker stack deploy --with-registry-auth -c docker-compose.yml brunch"'
                }
            }
        }
    }
}

