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

        stage('Quality Gates'){
            steps{
                script{
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar-token'
                }
            }
        }

        stage('Upload Artifac to Nexus'){
            steps{
                script{

                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def chooseNexusRepo = readPomVersion.version.endsWith ("SNAPSHOT") ? "counter-java-webapp-snapshot" : "counter-java-webapp-release"

                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot',
                            classifier: '',
                            file: 'target/Uber.jar',
                            type: 'jar'
                        
                        ]
                    ], 
                    credentialsId: 'nexus-cred', 
                    groupId: 'com.example', 
                    nexusUrl: '18.212.54.162:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: chooseNexusRepo, 
                    version: readPomVersion.version
                }
            }
        }
    }
}