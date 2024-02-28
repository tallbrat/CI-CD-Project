pipeline {
    agent none
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
        JFROG_PASSWORD  = credentials('jfrog-password')
    }
    stages {
        stage ('Maven as a agent') {
            agent {
                docker {
                    image 'maven:3.83'
                }
            }
            stage ('Compile Code') {
                steps {
                    sh "mvn compile"
                }
            }
            stage ('Run Unit Tests') {
                steps {
                    sh "mvn test"
                }
            }
            stage ('Create Build') {
                steps {
                    sh "mvn package"
                }
            }
        }
        post {
            failure {
                emailext body: 'Maven agent failed package the code', subject: 'Maven build failed', to: 'user@company.xyz'
        } 
        }   
    }
    stage ('Upload Binaries to Jfrog Artifactory') {
        rtServer (
            id: 'jfrog-server',
            url: 'http://172.25.205.170:8082/artifactory/',
            // If you're using username and password:
            //    username: 'user',
            //   password: 'password',
            // If you're using Credentials ID:
            credentialsId: 'artifactory',
            bypassProxy: true,
            timeout: 300
        )
        /*
        steps {
            sh ' jfrog rt upload --url http://192.168.1.230:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/*.jar java-web-app/'
        }
        */
        rtUpload (
            serverId: 'jfrog-server',
            spec: '''{
                "files": [
                    {
                    "pattern": "target/*.jar",
                    "target": "java-web-app/"
                    }
                ]
            }'''
        )
    }
    stage ('Download and Deploy to tomcat server') {
        steps {
            rtDownload (
                serverId: 'jfrog-server',
                spec: '''{
                    "files": [
                        {
                        "pattern": "java-web-app/",
                        "target": "/tmp"
                        }
                    ]
                }'''
            )
            sh 'sudo cp /tmp/*war /opt/tomcat/webapps/'
            sh 'sudo systemctl restart tomcat'
        }
    }

}
