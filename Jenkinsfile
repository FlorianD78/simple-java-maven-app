pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
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
        stage('Deliver') {
            steps{
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                sh 'NAME=`mvn help:evaluate -Dexpression=project.name`'
                sh 'VERSION=`mvn help:evaluate -Dexpression=project.version`'
                sh 'set -x'
                sh 'cd target'
                sh 'java -jar my-app-1.0-SNAPSHOT.jar'
            }
        }    
    }
}
