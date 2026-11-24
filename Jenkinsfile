pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Backend') {
            steps {
                dir('backend') { sh 'mvn clean package -DskipTests' }
            }
        }
        stage('Test Backend') {
            steps {
                dir('backend') { sh 'mvn test' }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir('backend') {
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.token=$SONAR_AUTH_TOKEN'                    }
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            }
        }
    }
}
