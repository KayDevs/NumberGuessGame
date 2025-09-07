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
                          -Dsonar.host.url=http://<EC2-Public-IP>:9000 \
                          -Dsonar.login=${SONAR_AUTH_TOKEN}
                    '''
        }
    }
}

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
    }
}

