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
        stage('Push to nexus') {
            steps{

                sh 'mvn dependency:resolve'
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                
                sh 'mvn deploy:deploy-file -DgroupId=asp.repo \
                                       -DartifactId=my-app-1.0-SNAPSHOT \
                                       -Dversion=1.0 \
                                       -Dpackaging=jar \
                                       -Dfile=target/my-app-1.0-SNAPSHOT.jar \
                                       -DgeneratePom=true \
                                       -DrepositoryId=labgorepo \
                                       -Durl=http://nexus.neosoft.asp/repository/labgorepo/'
                
                }
             }
        stage('Deploy'){
            steps{
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
            }
        }
    }
}

    
