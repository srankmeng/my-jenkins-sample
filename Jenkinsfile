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
                sh 'docker images'
            }
        }
    }
    post {
        always {
            echo 'pipeline done'
        }
    }
}
