pipeline {
    agent any

    stages {
        stage('Print Student Details') {
            steps {
                sh '''
                echo "Name: Albert Paul Sebastian"
                echo "Roll No: 2022BCS0007"
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m pip install --upgrade pip
                python3 -m pip install scikit-learn
                '''
            }
        }

        stage('Train ML Model') {
            steps {
                sh '''
                python3 - << EOF
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

data = load_iris()

X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2, random_state=42
)

model = RandomForestClassifier()
model.fit(X_train, y_train)

pred = model.predict(X_test)
accuracy = accuracy_score(y_test, pred)

print("Model Accuracy:", accuracy)
EOF
                '''
            }
        }
    }
}
