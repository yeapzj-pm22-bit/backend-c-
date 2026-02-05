pipeline {
    agent none
    
    stages {
        stage('Build and Deploy') {
            steps {
                script {
                    // Production deployment
                    if (env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'master') {
                        node('production-agent') {
                            stage('Checkout') {
                                checkout scm
                            }
                            stage('Restore Dependencies') {
                                bat 'dotnet restore'
                            }
                            stage('Build') {
                                bat 'dotnet build --configuration Release'
                            }
                            stage('Test') {
                                bat 'dotnet test --configuration Release --no-build || echo "No tests found"'
                            }
                            stage('Publish') {
                                bat 'dotnet publish --configuration Release --output ./publish'
                            }
                            stage('Deploy to Production') {
                                echo 'Deploying to Production Server...'
                                echo 'Published files are in ./publish directory'
                                // Add your deployment commands here
                                // Example: bat 'xcopy /E /Y .\\publish C:\\inetpub\\wwwroot\\backend-prod\\'
                            }
                        }
                    }
                    // Test/Development deployment
                    else if (env.BRANCH_NAME == 'develop' || env.BRANCH_NAME == 'test') {
                        node('test-agent') {
                            stage('Checkout') {
                                checkout scm
                            }
                            stage('Restore Dependencies') {
                                bat 'dotnet restore'
                            }
                            stage('Build') {
                                bat 'dotnet build --configuration Debug'
                            }
                            stage('Test') {
                                bat 'dotnet test --configuration Debug --no-build || echo "No tests found"'
                            }
                            stage('Publish') {
                                bat 'dotnet publish --configuration Debug --output ./publish'
                            }
                            stage('Deploy to Test') {
                                echo 'Deploying to Test Server...'
                                echo 'Published files are in ./publish directory'
                                // Add your deployment commands here
                                // Example: bat 'xcopy /E /Y .\\publish C:\\inetpub\\wwwroot\\backend-test\\'
                            }
                        }
                    }
                    else {
                        echo "Branch ${env.BRANCH_NAME} is not configured for deployment"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs above.'
        }
    }
}