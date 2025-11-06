pipeline {
    agent { label 'master' }

    tools {
        maven 'Maven-3.9.11'   // Name must match Manage Jenkins → Tools → Maven installations
    }

    environment {
        SONARQUBE = 'sonarqube'   // Name must match Manage Jenkins → System → SonarQube Servers
    }

    stages {

        stage('Build') {
            steps {
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" -B -DskipTests clean package'
            }
        }

        stage('Test') { 
            steps {
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    bat '"%MAVEN_HOME%\\bin\\mvn.cmd" sonar:sonar -Dsonar.projectKey=java-webapp'
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" clean deploy'
            }
        }

        stage('Done') {
            steps {
                echo '✅ Build, Test, Sonar, and Nexus Deploy completed successfully!'
            }
        }
    }
}
