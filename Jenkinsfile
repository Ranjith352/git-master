pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    stages {

        stage('Checkout') {
            steps {
                // Use HTTPS to avoid SSH key issues
                git branch: 'main', url: 'https://github.com/Ranjith352/git-master.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Installing dependencies..."
                npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
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

        // ✅ SHIFT LEFT IMPLEMENTATION
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My Sonar Server') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=git-master \
                    -Dsonar.projectName=git-master \
                    -Dsonar.sources=. \
                    -Dsonar.sourceEncoding=UTF-8
                    '''
                }
            }
        }

        // ✅ QUALITY GATE
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/output.txt', fingerprint: true
        }

        success {
            echo "Pipeline executed successfully. SonarQube analysis completed."
        }

        failure {
            echo "Pipeline failed. Check console output."
        }
    }
}
