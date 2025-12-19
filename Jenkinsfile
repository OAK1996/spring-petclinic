pipeline {
    agent any   // or: agent { label 'docker' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Static Analysis - SpotBugs') {
            steps {
                // Publish SpotBugs results (Warnings NG)
                // Adjust pattern if your SpotBugs report path differs
                recordIssues(
                    enabledForFailure: true,
                    tool: spotBugs(pattern: '**/target/spotbugs/spotbugsXml.xml')
                )
            }
        }

        stage('Package') {
            steps {
                // Optional: show generated jar
                sh 'ls -la target'
            }
        }
    }

    post {
        always {
            // Archive artifacts and SpotBugs reports for later inspection
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/spotbugs/*.xml', fingerprint: true
        }
    }
}
