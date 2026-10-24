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
	stage('Deploy Application') {
            steps {
                echo 'Deploying FastAPI application...'
                // The > /dev/null 2>&1 completely detaches the console output so Jenkins doesn't hang!
                sh '''
                    . venv/bin/activate
                    pkill uvicorn || true
                    nohup uvicorn main:app --host 0.0.0.0 --port 8000 > /dev/null 2>&1 &
                '''
            }
        }


    }
}

