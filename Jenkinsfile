pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    triggers {
        // Polling for GitHub PR webhooks only – avoid 'pollSCM' or 'cron'
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
                    env.IS_MAIN = (env.BRANCH_NAME == 'main').toString()
                }
            }
        }

        stage('CodeCoverage on Main') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh './gradlew test jacocoTestReport jacocoTestCoverageVerification'
                }
                script {
                    if (currentBuild.currentResult == 'FAILURE') {
                        echo 'tests fail!'
                    } else {
                        echo 'tests pass!'
                    }
                }
            }
            post {
                always {
                    junit '**/build/test-results/test/*.xml'
                    jacoco execPattern: '**/build/jacoco/test.exec'
                }
            }
        }

        stage('Other Tests on Non-Main') {
            when {
                not {
                    branch 'main'
                }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh './gradlew test integrationTest checkstyleMain'
                }
                script {
                    if (currentBuild.currentResult == 'FAILURE') {
                        echo 'tests fail!'
                    } else {
                        echo 'tests pass!'
                    }
                }
            }
            post {
                always {
                    junit '**/build/test-results/test/*.xml'
                    jacoco execPattern: '**/build/jacoco/test.exec'
                }
            }
        }
    }
}
