pipeline {
    agent any

    environment {
        HCP_AUTH_URL = 'https://auth.idp.hashicorp.com/oauth2/token'
        HCP_API_URL = 'https://api.cloud.hashicorp.com/secrets/2023-11-28/organizations/842e10da-396b-463e-b1fd-e143e5f632aa/projects/041d348e-0279-448d-9279-1c3e49d4bf31/apps/prikm/secrets:open'
    }

    stages {
        stage('Get HCP API Token') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'hcp-vault', usernameVariable: 'HCP_CLIENT_ID', passwordVariable: 'HCP_CLIENT_SECRET')]) {
                    script {
                        def tokenResponse = sh(
                            script: """curl --silent --location "$HCP_AUTH_URL" \
                            --header "Content-Type: application/x-www-form-urlencoded" \
                            --data-urlencode "client_id=$HCP_CLIENT_ID" \
                            --data-urlencode "client_secret=$HCP_CLIENT_SECRET" \
                            --data-urlencode "grant_type=client_credentials" \
                            --data-urlencode "audience=https://api.hashicorp.cloud" """,
                            returnStdout: true
                        ).trim()

                        def json = readJSON text: tokenResponse
                        env.HCP_API_TOKEN = json.access_token
                        echo "Access Token received"
                    }
                }
            }
        }

        stage('Read HCP Secrets') {
            steps {
                script {
                    withEnv(["HCP_API_TOKEN=${env.HCP_API_TOKEN}"]) {
                           def secretsResponse = sh(
                            script: """
                                set +x
                                curl --silent --location "$HCP_API_URL" \\
                                    --request GET \\
                                    --header "Authorization: Bearer \$HCP_API_TOKEN" 2>/dev/null
                                set -x
                            """,
                            returnStdout: true
                        ).trim()
                        echo "Secrets Response: ${secretsResponse}"
                        
                        def json = readJSON text: secretsResponse
                        def secrets = json.secrets
                    
                        secrets.each { secret ->
                            def name = secret.name
                            def value = secret.static_version?.value
                            echo "Secret name: ${name}, value: ${value}"
                        }
                    }
                }
            }
        }
    }
}
