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
            steps {
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
            }
            steps {
                sh 'NAME=`mvn help:evaluate -Dexpression=project.name'
            }
            steps {
                sh 'VERSION=`mvn help:evaluate -Dexpression=project.version'
            }
            steps {
                sh 'java -jar target/${NAME}-${VERSION}.jar'
            }

        }
    
    }
}

node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=simple-java-maven-app"
    }
  }
}
