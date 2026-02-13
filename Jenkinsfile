pipeline {
    agent any

    environment {
        // Match these to your Jenkins Global Tool and Credentials configuration
        SNYK_INST_NAME = 'Snyk@latest' 
        SNYK_TOKEN_ID  = 'snyk-api-token-id'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Your build commands (npm install, mvn package, etc.) go here
            }
        }

        stage('Security Test') {
            steps {
                echo 'Running Snyk Security Scan...'
                
                // This step is provided by the Snyk Jenkins Plugin
                snykSecurity(
                    snykInstallation: "${SNYK_INST_NAME}",
                    snykTokenId: "${SNYK_TOKEN_ID}",
                    additionalArguments: '--all-projects --detection-depth=3'
                )
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Your deployment logic goes here
            }
        }
    }
}
