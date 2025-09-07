pipeline {
    agent any

    tools {
        jdk 'Java-17'       // must match the name you added under "Manage Jenkins → Tools"
        maven 'Maven-3'     // must match the name you added under "Manage Jenkins → Tools"
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
                          -Dsonar.host.url=http://54.91.134.187:9000
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
                        path: '',
                        url: 'http://54.91.134.187:8080'
                    )
                ],
                contextPath: 'NumberGuessGame',
                war: 'target/NumberGuessGame-1.0-SNAPSHOT.war'
            }
        }
    }
}

