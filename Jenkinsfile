pipeline {
    agent any

    tools {
        // Must match Manage Jenkins > Tools > JDK
        jdk 'jdk-21' 
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    echo "Checking environment..."
                    sh 'java -version'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                // sh './mvnw clean install'
            }
        }

        stage('Security Scan') {
            steps {
                // FIX 2: Snyk plugin 'snykSecurity' step
                // Note: We use the ID directly here. The plugin handles the binding internally.
                snykSecurity(
                    snykInstallation: 'Snyk@latest', 
                    snykTokenId: 'organisational-snyk-api-token',
                    failOnIssues: true
                )
            }
        }

        stage('Deploy') {
            when { branch 'main' }
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        always {
            // FIX 3: Prevent "MissingContextVariableException"
            // We check if the workspace (FilePath) is still accessible before cleaning
            script {
                if (getContext(hudson.FilePath)) {
                    cleanWs()
                } else {
                    echo "Workspace not found, skipping cleanWs."
                }
            }
        }
        failure {
            echo "Pipeline failed. Check Snyk or Build logs above."
        }
    }
}
