pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '43.205.231.86'
        DEPLOY_PATH = '/home/ubuntu/cicd-demo' // Remote path where the app will be deployed
        SSH_KEY = credentials('Jenkins') // Jenkins credential ID for SSH key
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
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
                    // Copy files to the server
                    sh """
                        ssh -i ${SSH_KEY} ${DEPLOY_USER}@${DEPLOY_HOST} 'mkdir -p ${DEPLOY_PATH}'
                        rsync -avz --delete ./ ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}
                    """

                    // Restart the application on the server
                    sh """
                        ssh -i ${SSH_KEY} ${DEPLOY_USER}@${DEPLOY_HOST} 'cd ${DEPLOY_PATH} && npm install && pm2 restart all'
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
