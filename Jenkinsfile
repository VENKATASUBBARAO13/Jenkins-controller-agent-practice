// Example Jenkins Controller-Agent pipeline for learning and verification.
// Requires an online Linux agent with the label 'slave-1'.
// The Jenkins job should load this file from source control (Pipeline script from SCM).
// The Git checkout stage checks out this practice repository. Terraform stages
// are intentionally omitted because the repository does not contain Terraform files.

pipeline {
    agent { label 'slave-1' }

    options {
        timestamps()
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Agent') {
            steps {
                sh '''
                    echo "Running Jenkins job on:"
                    hostname
                    echo "Workspace: $WORKSPACE"
                    pwd
                '''
            }
        }

        stage('Verify Java and Git') {
            steps {
                sh '''
                    java -version
                    git --version
                '''
            }
        }

        stage('Check Terraform (Optional)') {
            steps {
                sh '''
                    if command -v terraform >/dev/null 2>&1; then
                        terraform version
                    else
                        echo "Terraform not installed on this agent. Skipping optional check."
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo 'Jenkins Controller-Agent practice pipeline completed successfully.'
        }
        failure {
            echo 'Jenkins pipeline failed. Review Console Output and the agent log.'
        }
        always {
            echo "Executed on node: ${env.NODE_NAME}"
        }
    }
}
