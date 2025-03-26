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
        
        stage('Deploy image') {
            steps {
                sh "docker run -d -p 80:80 tauruss/prikm"
            }
        }
    }
}