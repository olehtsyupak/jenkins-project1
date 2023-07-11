pipeline {
    agent {
        docker {
            image 'maven:3.9-alpine' 
            args '-v /root/.m2:/root/.m2'
        }
    }
    environment {
        APP_PORT = '9090'
        JOB_NAME = "${JOB_NAME}"
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Integration Test') {
            parallel {
                stage('Running Application') {
                    agent any
                    options {
                        timeout(time: 1, unit: 'MINUTES')
                    }
                    steps {
                        script {
                            try {
                                dir('target') {
                                    sh 'java -jar contact.war'
                                }
                            } catch (Exception e) {
                                echo "Caught exception: ${e}"
                                currentBuild.result = 'SUCCESS'
                            }
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                        sleep time: 30, unit: 'SECONDS'
                        sh 'mvn -Dtest=RestIT test'
                    }
                }
            }
        }
    }
}
