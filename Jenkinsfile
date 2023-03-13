pipeline {
    agent any

    stages {
        stage('Checkout From VCS'){
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/marcmael1/demo-counter-app.git']])
                }
            }
        }
    }
}