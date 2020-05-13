pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            parallel {
                stage('Test On Windows') {
                    agent {
                        label any
                    }
                    steps {
                        sh './jenkins/scripts/test.sh'
                    }
                    post {
                        always {
                            echo Windows
                        }
                    }
                }
                stage('Test On Linux') {
                    agent {
                        label any
                    }
                    steps {
                        sh './jenkins/scripts/test.sh'
                    }
                    post {
                        always {
                            echo linux
                        }
                    }
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
