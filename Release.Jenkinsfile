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
                    sh '''
                        # update service-chassis
                        mvn versions:use-releases -Dincludes="com.perrin:*" -s ${MAVEN_SETTINGS}
                        mvn versions:commit

                        # release               
                        mvn clean release:prepare -B -s ${MAVEN_SETTINGS} -Dresume=false
                        mvn release:perform -B -s ${MAVEN_SETTINGS}
                    '''
                }
            }
        }
    }
}