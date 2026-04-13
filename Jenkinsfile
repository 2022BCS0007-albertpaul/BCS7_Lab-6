pipeline {
    agent any

    stages {

        stage('Setup - Install Dependencies') {
            steps {
                sh '''
                    python3 -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Train Model') {
            steps {
                sh '''
                    python3 train.py
                '''
            }
        }

        stage('Identity') {
            steps {
                echo 'Student: Albert Paul Sebastian | Roll No: 2022BCS0007'
            }
        }

        stage('Archive Artifacts') {
            steps {
                sh '''
                    echo "Archiving model and metrics..."
                    ls -lah
                '''

                archiveArtifacts artifacts: 'model.pkl, metrics.json', fingerprint: true
            }
        }
    }
}