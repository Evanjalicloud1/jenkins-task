pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Evanjalicloud1/jenkins-task.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building project..."'
                sh 'ls -l'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                   echo "Deploying app to server..."
                   nohup python3 -m http.server 8081 >/dev/null 2>&1 &
                '''
            }
        }
    }
}
