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
                            credentialsId: 'fa4aa5a6-5908-41e3-8cf2-ed217a301ecd',
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
                sh 'sudo kubectl apply -f deployment.yml'
            }
        }
        }
    }
