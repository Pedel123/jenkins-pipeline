pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        // Add the JaCoCo Checkstyle stage
        stage('JaCoCo Checkstyle') {
            steps {
                // Run the Gradle task to verify test coverage
                sh './gradlew jacocoTestCoverageVerification'
            }
        }
    }

    post {
        always {
            // Add the publishHTML section here
            publishHTML(target: [
                reportDir: 'build/reports/jacoco/test/html', // Specify the path to the report
                reportFiles: 'index.html', // Specify the file to be published (JaCoCo HTML report)
                reportName: 'JaCoCo Checkstyle' // Display name of the report
            ])
        }
    }
}
