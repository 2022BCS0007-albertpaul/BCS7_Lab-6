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
                    cp -f model.pkl outputs/ 2>/dev/null || true
                    cp -f metrics.json outputs/ 2>/dev/null || true
                    echo "Artifacts prepared"
                '''
            }
        }
    }
}