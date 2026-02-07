pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'git@github.com:Ranjith352/git-master.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Installing dependencies..."
                npm install

                echo "Building Application..."
                rm -rf build
                mkdir build
                echo "Hello CI Pipeline" > build/output.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "Running Unit Tests..."
                npm test || true
                '''
            }
        }

        // ⭐⭐⭐ SHIFT-LEFT STAGE ⭐⭐⭐
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My Sonar Server') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=git-master \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=YOUR_TOKEN
                    '''
                }
            }
        }
    }

    post {
        always {
            junit '**/test-results.xml'
            archiveArtifacts artifacts: 'build/output.txt'
        }
    }
}
