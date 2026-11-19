pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checkout completed.'
                sh 'git rev-parse --short HEAD'
                sh 'ls -la'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'

                sh '''
                    set -eu

                    test -f index.html
                    test -f README.md

                    mkdir -p build

                    cp index.html build/index.html
                    cp README.md build/README.md

                    echo "Build completed successfully."
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the application...'

                sh '''
                    set -eu

                    test -s build/index.html
                    test -s build/README.md

                    grep -q "Source Code Version Control" build/index.html
                    grep -q "Build Automation Using Git and Jenkins" build/index.html

                    echo "All tests passed."
                '''
            }
        }

        stage('Archive') {
            steps {
                echo 'Archiving build artifacts...'

                archiveArtifacts(
                    artifacts: 'build/**',
                    fingerprint: true
                )

                echo 'Artifacts archived successfully.'
            }
        }
    }

    post {
        success {
            echo 'PROJECT STATUS: SUCCESS'
            echo 'Git and Jenkins pipeline completed successfully.'
        }

        failure {
            echo 'PROJECT STATUS: FAILED'
            echo 'Check the Console Output for errors.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}
