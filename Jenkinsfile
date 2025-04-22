pipeline {
    agent {
        docker {
            image 'openjdk:17'
        }
    }

    environment {
        DOCKER_REGISTRY = 'localhost:5001'
        REPO = 'repository'
        // Removed invalid BRANCH_NAME assignment
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh './gradlew test jacocoTestReport'
                    } else {
                        sh './gradlew test'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            when {
                allOf {
                    expression { env.BRANCH_NAME != 'playground' }
                    expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
                }
            }
            steps {
                script {
                    def imageName = env.BRANCH_NAME == 'main' ? 'calculator' : 'calculator-feature'
                    def version = env.BRANCH_NAME == 'main' ? '1.0' : '0.1'
                    def fullImage = "${DOCKER_REGISTRY}/${REPO}/${imageName}:${version}"

                    sh "docker build -t ${fullImage} ."
                    sh "docker push ${fullImage}"
                }
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/test/*.xml'
            script {
                if (fileExists('build/reports/jacoco/test/html/index.html')) {
                    echo 'Jacoco Report Generated'
                }
            }
        }
    }
}
