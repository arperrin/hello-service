pipeline {
    agent any
    tools {
        maven 'Maven 3.6.1'
    }
    stages { 
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                git 'https://github.com/arperrin/hello-service.git'
            }
        }
        stage('Release') {
            steps {                    
                configFileProvider([configFile(fileId: 'maven-settings', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS')]) {
                    sh 'mvn clean release:prepare release:perform -B -s ${MAVEN_SETTINGS}'
                }                
            }
        }
    }
}