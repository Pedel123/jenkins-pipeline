pipeline {
    agent {
        docker {
            image 'gradle:8.4.0-jdk17' // or whatever image suits your project
            args '-v $HOME/.gradle:/home/gradle/.gradle' // optional, for caching
        }
    }

    triggers {
        pollSCM('H/2 * * * *') // every 2 minutes (adjust as needed)
    }

    environment {
        JAVA_CHANGED = 'false'
    }

    stages {
        stage('Check Modified Files') {
            steps {
                script {
                    def changes = sh(script: "git diff --name-only HEAD~1 HEAD", returnStdout: true).trim()
                    if (changes =~ /\.java$/) {
                        env.JAVA_CHANGED = 'true'
                        echo "Java files were changed."
                    } else {
                        echo "No Java file changes."
                    }
                }
            }
        }

        stage('Run All Core Tests') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Run CodeCoverage and Checkstyle') {
            when {
                expression { env.JAVA_CHANGED == 'true' }
            }
            steps {
                sh './gradlew jacocoTestReport checkstyleMain'
            }
        }
    }

    post {
        success {
            echo 'pipeline ran perfectly'
        }
        failure {
            echo 'pipeline failure'
        }
    }
}
