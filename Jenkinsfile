pipeline {
    agent any

    stages {

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
                npm test
                '''
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
