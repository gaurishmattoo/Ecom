pipeline {
    agent any
    options { timestamps() }

    environment {
        VENV = '.venv'
        PORT = '5000'
        HOST = '65.2.140.173'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                    python3 -m venv ${VENV}
                    . ${VENV}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                    . ${VENV}/bin/activate
                    pkill -f "gunicorn.*:${PORT}" || true
                    nohup ${VENV}/bin/gunicorn -b 0.0.0.0:${PORT} app:app > flask.log 2>&1 &
                '''
                echo "App running at: http://${HOST}:${PORT}"
            }
        }
    }
}
