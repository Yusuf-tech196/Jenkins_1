cat > Jenkinsfile <<'EOF'
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
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh '''
                    set -eu
                    test -f index.html
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
                    grep -q "Source Code Version Control" build/index.html
                    grep -q "Build Automation Using Git and Jenkins" build/index.html
                    echo "All tests passed."
                '''
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/**',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'PROJECT STATUS: SUCCESS'
        }
        failure {
            echo 'PROJECT STATUS: FAILED'
        }
    }
}
EOF
