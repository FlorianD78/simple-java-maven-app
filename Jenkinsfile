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
                sh 'mvn dependency:resolve'
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
                sh 'echo **************Demarrage de SONARCLOUD**************'
                sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=FlorianD78_simple-java-maven-app'
            }
        }    
    }
}
