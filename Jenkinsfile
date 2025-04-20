pipeline {
    agent {
        docker {
            image 'gradle:8.5.0-jdk17'
            reuseNode true
            args '-v $HOME/.gradle:/home/gradle/.gradle'
        }
    }
    options {
        skipStagesAfterUnstable()
        timeout(time: 10, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *') // Check every minute for changes
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
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
    }
    post {
        always {
            archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
        }
    }
}
