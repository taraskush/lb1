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

        stage('Example') {
            steps {
                script {
                    ansiColor('xterm') {
                        echo "\u001B[32mThis is a green colored text\u001B[0m"
                    }
                }
            }
        }
    }
}
