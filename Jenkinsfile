pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "nexus.neosoft.asp"
        NEXUS_REPOSITORY = "lab.gorepo"
        NEXUS_CREDENTIAL_ID = "nexus-credentials"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B package --file pom.xml'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Push to nexus') {
            steps{

                sh 'mvn dependency:resolve'
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                
                
                }
             }
        }
        stage('Deploy'){
            steps{
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
            }
        }    
    }
}
