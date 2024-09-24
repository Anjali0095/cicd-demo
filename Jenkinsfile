pipeline {
    agent any

    stages {
        stage('Install Packages') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Run the App') {
            steps {
                script {
                    sh 'npm start &'
                    sleep 5
                }
            }
        }

        stage('Visit /health route') {
            steps {
                script {
                    sh 'curl http://localhost:3000/health'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'pkill -f "node"'
                }
            }
        }
    }
}
