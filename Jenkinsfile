pipeline {
    agent any

    stages {

        stage('Pull Latest Code') {
            steps {
                echo "Pulling latest code"
                sh '''
                cd /usr/src/Python-3.14.0/django-ec2
                git pull origin main
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                cd /usr/src/Python-3.14.0/django-ec2
                source venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Migrate Database') {
            steps {
                sh '''
                cd /usr/src/Python-3.14.0/django-ec2/config
                source ../venv/bin/activate
                python manage.py migrate
                '''
            }
        }

        stage('Collect Static') {
            steps {
                sh '''
                cd /usr/src/Python-3.14.0/django-ec2/config
                source ../venv/bin/activate
                python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Restart Gunicorn') {
            steps {
                sh '''
                sudo systemctl restart gunicorn
                '''
            }
        }

    }
}
