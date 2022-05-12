#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    stages {
        stage("build jar") {
            steps {
                script {
                    echo "building the application..."
                    sh 'mvn package'
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) 
                        sh 'docker build -t olehtsyupak/my-repo:jma-3.0 .'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker push olehtsyupak/my-repo:jma-3.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo 'deploying the application...'
                }
            }
        }
    }
}
