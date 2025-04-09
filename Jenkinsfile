pipeline {
    agent any
    
    parameters {
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment')
        string(name: 'PROJECT', defaultValue: 'app', description: 'Project')
    }

    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }

        stage('Send Notification') {
              steps {
                office365ConnectorSend message: """
Received parameters:\n
ENV: ${params.ENV}\n
PROJECT: ${params.PROJECT}
                """, webhookUrl: 'https://lpnu.webhook.office.com/webhookb2/0fd33370-9359-484e-b03a-237599a1a48e@7631cd62-5187-4e15-8b8e-ef653e366e7a/JenkinsCI/87977c05380247ff8ff305667672cbc9/80b3d679-e4be-4f50-b87f-820e8a770890/V2vjKJlWSSWP8TT1u0-_4fcHtPCTxsjH5N6X_jlSsKuj81'
            }
        }
    }

    post {
        success {
            publishHTML (target : [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'index.html',
                reportName: 'Lab_3.3',
                reportTitles: 'The Report'
            ])
        }
    }
}
