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
                sh 'cd target && pwd'
                sh 'pwd'
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=simple-java-maven-app -Dsonar.host.url=http://172.19.0.5:9000 -Dsonar.login=2f6b35c755a441228f7dca82c8dadc31158b8de3'
            }
        }    
    }
}
