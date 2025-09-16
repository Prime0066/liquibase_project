pipeline {
    agent any
    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-21' // Adjust to your Java 21 path
        PATH = "${JAVA_HOME}\\bin;C:\\Program Files\\liquibase;${PATH}" // Add Liquibase to PATH
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Verify Dependencies') {
            steps {
                dir('liquibase') {
                    script {
                        bat 'dir lib' // Debug: List files in lib directory
                        bat 'java -version'
                        bat 'liquibase --version' // Use installed Liquibase
                    }
                }
            }
        }
        stage('Database Update') {
            steps {
                dir('liquibase') {
                    script {
                        bat 'liquibase --defaultsFile=config/liquibase.properties update'
                    }
                }
            }
        }
        stage('Verify Database Status') {
            steps {
                dir('liquibase') {
                    script {
                        bat 'liquibase --defaultsFile=config/liquibase.properties status'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}