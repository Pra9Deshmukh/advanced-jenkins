pipeline {
    agent any

    options {
        // Keep last 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        // Timeout after 1 hour
        timeout(time: 1, unit: 'HOURS')
        // Add timestamps to console output
        timestamps()
    }

    environment {
        // Define environment variables here
        BUILD_USER = credentials('build-user-credentials')
        DEPLOY_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
                script {
                    env.GIT_COMMIT_MSG = sh(
                        script: "git log -1 --format=%B",
                        returnStdout: true
                    ).trim()
                    env.GIT_AUTHOR = sh(
                        script: "git log -1 --format=%an",
                        returnStdout: true
                    ).trim()
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                // Add your build commands here
                // Example for Maven: sh 'mvn clean package'
                // Example for Gradle: sh './gradlew build'
                // Example for Node.js: sh 'npm install && npm run build'
                sh '''
                    echo "Build stage"
                    # Add your build commands here
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    echo "Test stage"
                    # Add your test commands here
                    # Example: npm test, maven test, etc.
                '''
            }
            post {
                always {
                    // Publish test results
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality') {
            steps {
                echo 'Running code quality checks...'
                sh '''
                    echo "Code quality analysis"
                    # Add SonarQube or other analysis tools
                '''
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scans...'
                sh '''
                    echo "Security scanning"
                    # Add security scanning tools (OWASP, Trivy, etc.)
                '''
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to production...'
                sh '''
                    echo "Deployment stage"
                    # Add your deployment commands here
                '''
            }
        }

        stage('Post-Deploy Tests') {
            when {
                branch 'main'
            }
            steps {
                echo 'Running post-deployment tests...'
                sh '''
                    echo "Post-deployment tests"
                    # Add smoke tests or integration tests
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            // Clean workspace
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
            // Add notifications for successful builds
        }
        failure {
            echo 'Pipeline failed!'
            // Add notifications for failed builds
        }
        unstable {
            echo 'Pipeline is unstable!'
            // Handle unstable builds
        }
    }
}
