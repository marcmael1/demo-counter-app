pipeline {
    agent any

    stages {
        stage('Checkout From VCS'){
            steps{
                script{
                    checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/marcmael1/demo-counter-app.git']])
                }
            }
        }

        stage('Unit-Test'){
            steps{
                script{
                    sh 'mvn test'
                }
            }
        }

        stage('Integration-Test'){
            steps{
                script{
                    sh 'mvn verify -DskipUnit-Test'
                }
            }
        }

        stage('Build Artifac'){
            steps{
                script{
                    sh 'mvn clean install'
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}