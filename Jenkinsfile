pipeline {

    agent {
        node {
            label 'docker-agent-alpine'
        }
    }

    triggers {
        pollSCM('* * * * *')
    }

    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    stages {

        stage('Build') {
            steps {
                echo "Building.. #${BUILD_ID}"
                sh '''
                    cd myapp
                    pip3 install -r requirements.txt --break-system-packages
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing.. #${BUILD_ID}"
                sh '''
                    cd myapp
                    python3 hello.py
                    python3 hello.py --name=Brad
                '''
            }
        }

        stage('Deliver') {
            steps {
                echo "Delivering.. #${BUILD_ID}"
                sh '''
                    echo "Delivery done successfully!"
                '''
            }
        }

    }

    post {
        success {
            echo "Build #${BUILD_ID} passed ✅"
        }
        failure {
            echo "Build #${BUILD_ID} failed ❌"
        }
        always {
            cleanWs()
        }
    }

}
