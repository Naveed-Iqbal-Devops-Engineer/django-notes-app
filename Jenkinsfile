pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Naveed-Iqbal-Devops-Engineer/django-notes-app.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Migrations') {
            steps {
                sh '''
                    . venv/bin/activate
                    python manage.py makemigrations
                    python manage.py migrate
                '''
            }
        }

        stage('Collect Static Files') {
            steps {
                sh '''
                    . venv/bin/activate
                    python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Run Django App with Gunicorn') {
            steps {
                sh '''
                    . venv/bin/activate
                    gunicorn notesapp.wsgi:application --bind 0.0.0.0:8000 --workers 3
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful. Django app running on port 8000 (using Gunicorn).'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}

