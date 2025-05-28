pipeline {
    agent any
    
    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }
        
        stage('Image build') {
            steps {
                sh "docker build -t prikm:latest ."
                sh "docker tag prikm tauruss/prikm:latest"
                sh "docker tag prikm tauruss/prikm:$BUILD_NUMBER"
            }
        }
        
        stage('Push to registry') {
            steps {
                withDockerRegistry([credentialsId: "dh_token", url: ""]) {
                    sh "docker push tauruss/prikm:latest"
                    sh "docker push tauruss/prikm:$BUILD_NUMBER"
                }
            }
        }

        stage('Stop running container') {
            steps {
                script {
                    def containerId = sh(script: "docker ps -q -f 'publish=80'", returnStdout: true).trim()
                    if (containerId) {
                        echo "Stopping running container on port 80: $containerId"
                        sh "docker stop $containerId"
                    } else {
                        echo "No container running on port 80"
                    }
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                sh "docker run -d -p 80:80 tauruss/prikm"
            }
        }
    }
}
