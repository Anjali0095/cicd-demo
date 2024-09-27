pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        DEPLOY_PATH = '/home/ubuntu/cicd-demo'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Specify the branch to checkout
                    git url: 'https://github.com/Anjali0095/cicd-demo.git', credentialsId: 'github', branch: 'master'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm ci' // Using npm ci for production
                }
            }
        }

        stage('Build the App') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    // Create the deployment directory if it doesn't exist
                    sh "mkdir -p ${DEPLOY_PATH}"

                    // Use rsync to sync build files to the deployment path
                    sh "rsync -avz --delete ./build/ ${DEPLOY_PATH}/"

                    // Install dependencies and restart the application using pm2
                    sh """
                        cd ${DEPLOY_PATH} && npm ci && pm2 restart all
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
