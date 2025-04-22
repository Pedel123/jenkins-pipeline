pipeline {
    agent any

    stages {
        stage('Tests for Non-Main Branches') {
            when {
                not {
                    branch 'main'
                }
            }
            steps {
                script {
                    try {
                        sh './gradlew test integrationTest staticAnalysis'
                        echo 'tests pass!'
                    } catch (err) {
                        echo 'tests fail!'
                    } finally {
                        sh './gradlew jacocoTestReport'
                    }
                }
            }
        }

        stage('Code Coverage for Main Branch') {
            when {
                branch 'main'
            }
            steps {
                script {
                    try {
                        sh './gradlew codeCoverageTest'
                        echo 'tests pass!'
                    } catch (err) {
                        echo 'tests fail!'
                    } finally {
                        sh './gradlew jacocoTestReport'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/reports/jacoco/test/html/**', allowEmptyArchive: true
        }
    }
}
