pipeline {
    agent { label 'master' }

    // 👇 This tells Jenkins to use the Maven you defined in "Global Tool Configuration"
    tools {
        maven 'Maven-3.9.11'
    }

    stages {
        stage('Build') {
            steps {
                // Use %MAVEN_HOME% so it always finds Maven even when Jenkins runs as service
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

        stage('Sonar-Report') {
            steps {
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" clean install sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.analysis.mode=publish'
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
