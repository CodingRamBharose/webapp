pipeline {
    agent { label 'master' }

    tools {
        maven 'Maven-3.9.11'
    }

    environment {
        SONARQUBE = 'SonarQube'  // Must match Jenkins → System → SonarQube Servers → Name
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
                withSonarQubeEnv("${env.SONARQUBE}") {
                    bat '"%MAVEN_HOME%\\bin\\mvn.cmd" sonar:sonar -Dsonar.projectKey=java-webapp'
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                // THIS IS THE LINE WE CHANGED:
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" -s "C:\\Users\\codingrambharose\\.m2\\settings.xml" clean deploy'
            }
        }

        stage('Done') {
            steps {
                echo '✅ Build, Test, Sonar, and Nexus Deploy completed successfully!'
            }
        }
    }
}