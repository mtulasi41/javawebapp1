pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Mven3.9"
    }

    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-credential', url: 'https://github.com/mtulasi41/Jenkins-kops-deployment.git']])
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        }
        stage('build-notify') {
            steps {
                slackSend channel: 'opstream', message: 'Build success', teamDomain: 'creativework-co', tokenCredentialId: 'slack'
            }
        }
        stage('Code Quality') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean install -f pom.xml sonar:sonar'
                }
            }
        }
       stage('Prod Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://13.235.98.142:8080/')], contextPath: null, war: '**/*.war'
            }
        }
        stage('Deploy-notify') {
            steps {
                slackSend channel: 'opstream', message: 'Deployment Success', teamDomain: 'creativework-co', tokenCredentialId: 'slack'
            }
        }
    }
}
