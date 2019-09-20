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
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                     configFileProvider([configFile(fileId: 'maven-settings', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS')]) {
                        sh '''
                            # update service-chassis
                            mvn versions:use-releases -Dincludes="com.perrin:*" -s ${MAVEN_SETTINGS}
                            mvn versions:commit

                            # commit
                            git add .
                            git commit -m '[JENKINS]: Remove SNAPSHOT from dependencies prior to release.'
                            git push

                            # release               
                            mvn clean release:prepare -B -s ${MAVEN_SETTINGS} -Dresume=false
                            mvn release:perform -B -s ${MAVEN_SETTINGS}

                            # update service-chassis to next snapshot
                            mvn versions:use-latest-snapshots -D

                            # commit       
                            git add .
                            git commit -m '[JENKINS]: Updating dependency versions to latest development snapshots post-release'
                            git push
                        '''
                    }         
                }
            }
        }
    }
}