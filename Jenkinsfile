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

        // ⭐ SHIFT-LEFT STAGE
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My Sonar Server') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=git-master \
                    -Dsonar.sources=. \
                    -Dsonar.sourceEncoding=UTF-8
                    '''
                }
            }
        }

        // ⭐ OPTIONAL BUT HIGHLY RECOMMENDED
        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
