pipeline {
    agent any

    parameters {
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment')
        string(name: 'PROJECT', defaultValue: 'app', description: 'Project')
    }

    options {
        ansiColor('xterm')
    }
    stages {
        stage('Build') {
            steps {
                echo '\033[34m${params.ENV}\033[0m \033[33m${params.PROJECT}\033[0m'
            }
        }
    }
}