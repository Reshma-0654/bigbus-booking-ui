pipeline {

    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'github_creds',
                        url: 'https://github.com/Reshma-0654/bigbus-booking-ui.git'
                    ]]
                )
            }
        }

        stage('Validate') {
            steps {
                echo 'Validating Maven project'
                sh 'mvn validate'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling project'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application'
                sh 'mvn package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving WAR file'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying WAR file to Tomcat'
                sh 'cp target/*.war /opt/tomcat/webapps/'
            }
        }
    }

    post {

        success {
            echo 'Build Successful'
        }

        failure {
            echo 'Build Failed'
        }

        always {
            echo 'Pipeline Completed'
        }
    }
}
