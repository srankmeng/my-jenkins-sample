pipeline {
    agent any

    stages {
        stage('Checkout code') {
            // steps {
            //     checkout scm
            // }
            steps {
              git branch: 'main', url: 'https://github.com/srankmeng/my-jenkins-sample.git'
            }
        }
        stage('Code analysis') {
            steps {
                echo 'Code analysis'
            }
        }
        stage('Unit test') {
            steps {
                echo 'Unit test'
            }
        }
        stage('Code coverage') {
            steps {
                echo 'Code coverage'
            }
        }
        stage('Build images') {
            steps {
                sh 'docker build -f ./json-server/Dockerfile -t srank123/json-server:0.1.0 ./json-server'
            }
            // steps {
            //     sh 'docker compose build json-server'
            // }
        }
        stage('Setup & Provisioning') {
            steps {
                sh 'docker run -p 3000:3000 --name json-server -d srank123/json-server:0.1.0'
            }
            // steps {
            //     sh 'docker compose up json-server -d'
            // }
        }
        stage('Run api automate test') {
            steps {
                sh 'docker build -f ./newman/Dockerfile -t srank123/newman:0.1.0 ./newman'
                sh 'docker run srank123/newman:0.1.0'
            }
            // steps {
            //     sh 'docker compose up newman'
            // }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub'
                , passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh '''docker image tag srank123/json-server:0.1.0 srank123/json-server:$BUILD_NUMBER
                          docker image push srank123/json-server:$BUILD_NUMBER'''
                }        
            }
        }
    }
    post {
        always {
            sh 'docker rm -f json-server'
            // sh 'docker compose down'
        }
        success {
            script {
                def currentBuildNumber = env.BUILD_NUMBER
                build job: 'demo_deploy_pipeline', parameters: [string(name: 'IMAGE_TAG', value: "${currentBuildNumber}")]
            }
        }
    }
}
