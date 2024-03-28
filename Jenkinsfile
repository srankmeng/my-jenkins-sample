pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
            // steps {
            //   git branch: 'main', url: 'https://github.com/srankmeng/my-jenkins-sample.git'
            // }
        }
    }
    post {
        always {
            echo 'pipeline done'
        }
    }
}
