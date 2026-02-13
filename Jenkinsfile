pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "albertpaulbcs7/lab6-model"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                sh '''
                python3 -m venv .venv
                . .venv/bin/activate
                pip install --break-system-packages -r requirements.txt
                '''
            }
        }

        stage('Train Model') {
            steps {
                sh '''
                . .venv/bin/activate
                python scripts/train.py
                '''
            }
        }

        stage('Read Accuracy') {
            steps {
                script {
                    def acc = sh(
                        script: "jq -r .accuracy app/artifacts/metrics.json",
                        returnStdout: true
                    ).trim()

                    env.CURRENT_ACCURACY = acc
                    echo "Current Accuracy: ${acc}"
                }
            }
        }

        stage('Compare Accuracy') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'BEST_ACCURACY', variable: 'BEST')]) {
                        def result = sh(
                            script: "echo \"${env.CURRENT_ACCURACY} > ${BEST}\" | bc",
                            returnStdout: true
                        ).trim()

                        env.IS_BETTER = result
                        echo "Is Better: ${result}"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            when {
                expression { env.IS_BETTER == '1' }
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker build -t $DOCKER_IMAGE:${BUILD_NUMBER} .
                    docker tag $DOCKER_IMAGE:${BUILD_NUMBER} $DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression { env.IS_BETTER == '1' }
            }
            steps {
                sh '''
                docker push $DOCKER_IMAGE:${BUILD_NUMBER}
                docker push $DOCKER_IMAGE:latest
                '''
            }
        }
    }
}
