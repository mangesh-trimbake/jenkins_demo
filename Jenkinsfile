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
                echo 'Deploying FastAPI application securely...'
                sh '''
                    # 1. Enter environment
                    . venv/bin/activate
                    
                    # 2. Stop any old running instance cleanly
                    pkill uvicorn || true
                    
                    # 3. CRUCIAL: Protect the process from the Jenkins Process Tree Killer
                    export JENKINS_NODE_COOKIE=dontKillMe
                    
                    # 4. Fire up uvicorn in the background using direct workspace log anchoring
                    nohup uvicorn main:app --host 0.0.0.0 --port 8000 > "${WORKSPACE}/uvicorn.log" 2>&1 &
                    
                    # 5. Let Jenkins pause momentarily to ensure it didn't immediate crash
                    sleep 3
                '''
            }
        }
	


    }
}

