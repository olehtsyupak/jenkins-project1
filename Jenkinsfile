#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/olehtsyupak/jenkins-shared-library.git',
         credentialsId: 'github-credentials'
        ]
)

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    environment {
        IMAGE_NAME = 'olehtsyupak/my-repo:java-maven-1.0'
    }

    stages {
            stage('build app') {
                steps {
                   script {
                      echo 'building application jar...'
                      buildJar()
                   }
                }
            }
            stage('build image') {
                    steps {
                        script {
                           echo 'building docker image...'
                           buildImage(env.IMAGE_NAME)
                           dockerLogin()
                           dockerPush(env.IMAGE_NAME)
                    }
                }
            }
            stage('deploy') {
                    steps {
                        script {
                            echo 'deploying docker image to EC2...'
                            
                            def shellCmd = "bash ./server-cmds.sh"
                            
                            sshagent(['ec2-server-key']) {
                                sh "scp server-cmds.sh ec2-user@18.197.183.190:/home/ec2-user"
                                sh "ssh -o StrictHostKeyChecking=no ec2-user@18.197.183.190 ${shellCmd}"
                            }
                        }
                    }
                }
            }
        }
        