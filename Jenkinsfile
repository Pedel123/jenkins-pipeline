pipeline {
    agent any
    triggers {
        githubPullRequest() // Requires GitHub plugin and GitHub branch source plugin
    }
    environment {
        JACOCO_REPORT = "build/reports/jacoco/test/html/index.html"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Determine Branch') {
            steps {
                script {
                    IS_MAIN = env.BRANCH_NAME == 'main'
                    echo "Running on branch: ${env.BRANCH_NAME}"
                    echo "Is main branch: ${IS_MAIN}"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        if (IS_MAIN) {
                            echo "Running CodeCoverage test on main branch"
                            sh './gradlew jacocoTestReport'
                        } else {
                            echo "Running unit tests, integration tests, and static analysis"
                            sh './gradlew test integrationTest checkstyleMain checkstyleTest'
                            sh './gradlew jacocoTestReport'
                        }
                        echo "tests pass!"
                    } catch (Exception err) {
                        echo "tests fail!"
                        sh './gradlew jacocoTestReport || true'
                        throw err
                    }
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build/reports/jacoco/test/html/**', fingerprint: true
        }
    }
}
