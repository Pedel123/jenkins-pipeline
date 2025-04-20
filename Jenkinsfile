pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Run Tests') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "Running CodeCoverage on main branch"
                    // Add your CodeCoverage command here, e.g., ./gradlew jacocoTestCoverageVerification
                }
            }
        }
        stage('Run Other Tests') {
            when {
                branch 'feature/*'
            }
            steps {
                script {
                    echo "Running all tests except CodeCoverage on feature branch"
                    // Add commands for other tests here, e.g., ./gradlew test checkstyle
                }
            }
        }
        stage('Fail Pipeline') {
            when {
                not {
                    anyOf {
                        branch 'main'
                        branch 'feature/*'
                    }
                }
            }
            steps {
                error "This branch is neither 'main' nor 'feature'. Failing the pipeline."
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
