pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/celinechege/8.2CDevSecOp.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: '281be69cf4e0b0a4ea61aa5e0252e390d64d016c', variable: '281be69cf4e0b0a4ea61aa5e0252e390d64d016c')]) {
                sh '''
                # Download SonarScanner CLI
                curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                unzip -o sonar-scanner.zip
                export PATH=$PWD/sonar-scanner-5.0.1.3006-linux/bin:$PATH

                # Run SonarScanner
                sonar-scanner
                '''
                }
            }
        }

    }
}


