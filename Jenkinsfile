pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'git@github.com:Ranjith352/git-master.git'
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
                npm install
                npm test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results.xml'
            archiveArtifacts artifacts: 'build/output.txt'
        }
    }
}
