pipeline {
    agent any

    stages {

        stage('Setup Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Train Model') {
            steps {
                sh '''
                    . venv/bin/activate
                    python scripts/train.py
                '''
            }
        }

        stage('Identity Print') {
            steps {
                echo 'Student: Albert Paul Sebastian | Roll No: 2022BCS0007'
            }
        }

        stage('Archive Output') {
            steps {
                sh '''
                    . venv/bin/activate
                    mkdir -p outputs

                    echo "Copying artifacts from app/artifacts..."

                    cp -f app/artifacts/model.pkl outputs/ 2>/dev/null || true
                    cp -f app/artifacts/metrics.json outputs/ 2>/dev/null || true

                    echo "Final output directory:"
                    ls -R outputs || true
                '''

                archiveArtifacts artifacts: 'outputs/**', fingerprint: true
            }
        }
    }
}