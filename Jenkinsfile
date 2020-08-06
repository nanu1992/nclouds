pipeline {
    agent any
    environment {
        //variable for image name
        PROJECT = "pytestproject"
    }
    stages {
        stage('Initialize') {
            steps {
                deleteDir()
            }
        }
        stage('Clean Workspace') {
            steps {
                checkout scm
            }
        }
        stage ('Build and Push') {
            steps {
                            withCredentials([[$class: 'UsernamePasswordMultiBinding', 
                            credentialsId: 'f6857cca-9128-4de3-9a0b-a4f12d56a723',
                            usernameVariable: 'REGISTRY_USER', 
                            passwordVariable: 'REGISTRY_PASSWORD']]) {
                        sh 'docker build -t ${REGISTRY_USER}/${PROJECT} .'
                        sh 'docker login -u ${REGISTRY_USER} -p ${REGISTRY_PASSWORD} '
                        sh 'docker push ${REGISTRY_USER}/${PROJECT}'
                    }
                }
        }
        stage ('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
            }
        }
        }
    }
