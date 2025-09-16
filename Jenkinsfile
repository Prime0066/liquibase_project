pipeline {
    agent any
    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-21' // Adjust to your Java installation path
        PATH = "${JAVA_HOME}\\bin;${PATH}"
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
                        bat 'java -jar lib/liquibase.jar --version'
                    }
                }
            }
        }
        stage('Database Update') {
            steps {
                dir('liquibase') {
                    script {
                        bat 'java -jar lib/liquibase.jar --defaultsFile=config/liquibase.properties update'
                    }
                }
            }
        }
        stage('Verify Database Status') {
            steps {
                dir('liquibase') {
                    script {
                        bat 'java -jar lib/liquibase.jar --defaultsFile=config/liquibase.properties status'
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