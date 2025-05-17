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
    withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
      sh '''
        export SONAR_SCANNER_VERSION=7.0.2.4839
        export SONAR_SCANNER_HOME=$HOME/.sonar/sonar-scanner-$SONAR_SCANNER_VERSION-macosx-x64
        curl --create-dirs -sSLo $HOME/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION-macosx-x64.zip
        unzip -o $HOME/.sonar/sonar-scanner.zip -d $HOME/.sonar/
        export PATH=$SONAR_SCANNER_HOME/bin:$PATH
        export SONAR_SCANNER_OPTS="-server"

        sonar-scanner \
          -Dsonar.organization=celinechege \
          -Dsonar.projectKey=celinechege_8.2CDevSecOp \
          -Dsonar.sources=. \
          -Dsonar.host.url=https://sonarcloud.io \
          -Dsonar.login=$SONAR_TOKEN
      '''
    }
  }
  }

    }
}


