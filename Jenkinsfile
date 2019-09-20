pipeline {
    agent any
    tools {
        maven 'Maven 3.6.1'
    }
    stages { 
        stage('Checkout') {
            steps {
                git 'https://github.com/arperrin/hello-service.git'
            }
        }
        stage('Build') {
            steps {                
                configFileProvider([configFile(fileId: 'maven-settings', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS')]) {
                    sh 'mvn clean deploy -s ${MAVEN_SETTINGS}'
                }                
            }
        }
    }
}