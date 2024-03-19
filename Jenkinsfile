#!/usr/bin/env groovy

import groovy.transform.Field

@Field
String DOCKER_USER_REF = 'v-docker-hub'
@Field
String SSH_ID_REF = 'ssh-credentials-id'

pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage("build and test") {
            steps {
                sh "ls -la"
                sh "docker build -t vitnguyen/mgm-training-todo-app:0.0.2 ."
                echo 'code build'
            }
        }
        stage("Docker login and push docker image") {
            steps {
                withBuildConfiguration {
                    sh 'docker login --username ${repository_username} --password ${repository_password}'
                    sh "docker push vitnguyen/mgm-training-todo-app:0.0.2"
                }
            }
        }
        stage("deploy") {
            steps {
                withBuildConfiguration {
                    sshagent(credentials: [SSH_ID_REF]) {
                        sh '''
                            ssh root@ec2-18-141-234-249.ap-southeast-1.compute.amazonaws.com "docker run --detach --name vitnguyen-todo-app -p 8000:8000 vitnguyen/mgm-training-todoapp:0.0.2"
                            ssh root@ec2-18-142-231-213.ap-southeast-1.compute.amazonaws.com "docker run --detach --name vitnguyen-todo-app -p 8000:8000 vitnguyen/mgm-training-todoapp:0.0.2"
                        '''
                    }
                }
            }
        }
    }
}

void withBuildConfiguration(Closure body) {
    withCredentials([usernamePassword(credentialsId: DOCKER_USER_REF, usernameVariable: 'repository_username', passwordVariable: 'repository_password')]) {
        body()
    }
}
