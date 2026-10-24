pipeline {
    agent any

    stages {
        stage('Setup Virtual Environment & Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Automated Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest test_main.py -v
                '''
            }
        }
    }
}

