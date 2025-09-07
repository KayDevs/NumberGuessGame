pipeline {
    agent any

    tools {
        jdk 'Java-17'
        maven 'Maven-3'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('MySonar') {
                    sh '''
                        mvn sonar:sonar \
                          -Dsonar.projectKey=NumberGuessGame \
                          -Dsonar.host.url=http://54.91.134.187:9000 || true
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'tomcat-cred',
                        url: 'http://54.91.134.187:8081/manager/text',
                        path: '/NumberGuessGame'
                    )
                ],
                war: 'target/NumberGuessGame-1.0-SNAPSHOT.war'
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    // Wait a few seconds for Tomcat to deploy the WAR
                    sleep 10

                    // Check if the application responds with HTTP 200
                    def response = sh(
                        script: 'curl -o /dev/null -s -w "%{http_code}" http://54.91.134.187:8081/NumberGuessGame/',
                        returnStdout: true
                    ).trim()

                    if (response != '200') {
                        error "Deployment failed! HTTP response code: ${response}"
                    } else {
                        echo "Deployment successful! Application is running."
                    }
                }
            }
        }
    }
}

