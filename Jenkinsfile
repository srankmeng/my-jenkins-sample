pipeline {
    // environment {
    //     BUILD_NUM = ''
    // }
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
                // script {
                //     BUILD_NUM = '$BUILD_NUMBER'
                //     echo BUILD_NUM
                // }
                
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
        // stage('Trigger deploy') {
        //     steps {
        //         // script {
        //         //     IMAGE_TAG = '$BUILD_NUMBER'
        //         //  }
        //         //  sh "echo ${IMAGE_TAG}"
        //         // build job: 'demo_deploy_pipeline', parameters: [string(name: 'IMAGE_TAG', value: '${IMAGE_TAG}')]    
        //         build job: 'demo_deploy_pipeline'
        //     }
        // }
    }
    post {
        success {
            script {
                      BUILD_NUM = '$BUILD_NUMBER'
                 }
                 sh "echo ${BUILD_NUM}"
            build job: 'demo_deploy_pipeline', parameters: [string(name: 'IMAGE_TAG', value: '${BUILD_NUM}')]
        }
    }
}
