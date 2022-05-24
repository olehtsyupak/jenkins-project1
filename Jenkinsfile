#!/usr/bin/env groovy

@Library('jenkins-shared-library')

def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    buildImage 'olehtsyupak/my-repo:jma-4.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}

