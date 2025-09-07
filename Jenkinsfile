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

