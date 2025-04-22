pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        // This is a pull request
                        echo 'Running tests for PR...'
                        // Add your test commands here
                    } else {
                        // This is a regular branch
                        echo 'Running tests for regular branch...'
                        // Add your test commands here
                    }
                }
            }
        }
    }
}
