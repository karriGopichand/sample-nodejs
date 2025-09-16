pipeline {
    agent { label 'agent-online' }   // Jenkins agent label

    environment {
        APP_DIR = "/home/ubuntu/sample-nodejs"
        APP_NAME = "sample-nodejs"   // PM2 process name
    }

    stages {
        stage('Checkout') {
            steps {
                dir("${APP_DIR}") {
                    git branch: 'main', url: 'https://github.com/karriGopichand/sample-nodejs.git'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir("${APP_DIR}") {
                    sh 'npm install'
                }
            }
        }

        stage('Run Application with PM2') {
            steps {
                dir("${APP_DIR}") {
                    sh '''
                        # Stop old instance if running
                        pm2 delete $APP_NAME || true

                        # Start new instance with PM2
                        pm2 start ./bin/www --name $APP_NAME

                        # Save PM2 process list
                        pm2 save

                        # Ensure PM2 restarts on reboot (systemd)
                        
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                sh 'pm2 status $APP_NAME'
            }
        }
    }
}

