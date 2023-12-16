pipeline {
    agent {
        docker {
            image 'node:16-buster' 
            args '-p 3000:3000' 
        }
    }
    environment {
        PUBLIC_URL       = 'https://Satriadiouf24.github.io/a428-cicd-labs'
        GITHUB_TOKEN     = credentials('jenkins-github-token')
        GITHUB_REPOSITORY = 'Satriadiouf24/a428-cicd-labs'
    }
    stages {
         stage('Checkout') {
      steps {
        script {
          checkout([$class: 'GitSCM', branches: [[name: '*/react-app']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Satriadiouf24/a428-cicd-labs', credentialsId: GITHUB_TOKEN]]])
        }
      }
    }
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                sh './jenkins/scripts/kill.sh'
                sh 'chmod +x ./jenkins/scripts/github-pages.sh && ./jenkins/scripts/github-pages.sh'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build/**/*', fingerprint: true
        }
    }
}
