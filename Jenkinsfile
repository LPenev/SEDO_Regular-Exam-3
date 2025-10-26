pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
        // githubPush()
    }

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        PATH = "${DOTNET_ROOT}:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                echo 'Restoring NuGet packages...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'dotnet test --configuration Release --no-build --logger trx'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing build artifacts...'
                sh 'dotnet publish -c Release -o published'
            }
        }
    }

    post {
        success {
            echo 'Build and tests completed successfully!'
            archiveArtifacts artifacts: 'published/**/*', fingerprint: true
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
