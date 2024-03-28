pipeline {
    agent any

    stages {
        // stage('Checkout') {
        //     steps {
        //         checkout scm
        //     }
        //     steps {
        //       git branch: 'main', url: 'https://github.com/srankmeng/my-jenkins-sample.git'
        //     }
        // }
        stage('Code analytic') {
            steps {
                echo 'Code analytic'
            }
        }
        stage('Unit test') {
            steps {
                echo 'Unit test'
            }
        }
        stage('Build images') {
            steps {
                sh 'docker build -f ./json-server/Dockerfile -t json-server:0.1.0 ./json-server'
                sh 'docker tag json-server:0.1.0 srank123/json-server:$BUILD_NUMBER'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub'
                , passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker image push srank123/json-server:$BUILD_NUMBER'
                }        
            }
        }
    }
    post {
        success {
            build 'demo_deploy_pipeline'
        }
    }
}
