pipeline {
    agent any

    tools {
        maven 'Apache-Maven'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/NV-Narayana/calcwebapp.git'

                echo 'Code Checked-out Successfully!!'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn clean package'

                echo 'Maven Package Goal Executed Successfully!'
            }
        }

        stage('JUnit Reports') {
            steps {
                junit 'target/surefire-reports/*.xml'

                echo 'Publishing JUnit Reports'
            }
        }

        stage('Jacoco Reports') {
            steps {
                jacoco()

                echo 'Publishing Jacoco Code Coverage Reports'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh '''
                        mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.5.0.6356:sonar
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
