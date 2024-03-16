#!/usr/bin/env groovy

pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage("build and test"){
            steps{
                sh "ls -la"
                sh "docker build -t mgm-training-todo-app ."
                echo 'code build'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning'
            }
        }
        stage("push"){
            steps{
                echo 'Push docker image'
            }
        }
        stage("deploy"){
            steps{
                echo 'deployment'
            }
        }
    }
//    post {
//        // Clean after build
//        always {
//            cleanWs(cleanWhenNotBuilt: false,
//                    deleteDirs: true,
//                    disableDeferredWipeout: true,
//                    notFailBuild: true)
//        }
//    }
}
