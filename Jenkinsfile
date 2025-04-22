pipeline {
    agent any
    options {
        skipDefaultCheckout()
    }
    triggers {
        githubPullRequest()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Conditional Tests') {
            steps {
                script {
                    def branch = env.BRANCH_NAME

                    if (branch == 'main') {
                        echo "Running CodeCoverage on main branch"
                        try {
                            sh './gradlew jacocoTestReport'
                            echo "tests pass!"
                        } catch (err) {
                            echo "tests fail!"
                        }
                    } else {
                        echo "Running unit, integration, and static analysis on ${branch}"
                        try {
                            sh './gradlew test integrationTest checkstyleMain checkstyleTest jacocoTestReport'
                            echo "tests pass!"
                        } catch (err) {
                            echo "tests fail!"
                        }
                    }
                }
            }
        }

        stage('Publish Jacoco Report') {
            steps {
                jacoco execPattern: '**/build/jacoco/*.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java', exclusionPattern: ''
            }
        }
    }
}
