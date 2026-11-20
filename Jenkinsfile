pipeline {
    agent any

    //  MANDATORY for the GitHub webhook trigger to work
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code checked out successfully!'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        // --- SONARQUBE STAGE ---
        stage('SonarQube Analysis') {
            steps {
                // 'techie-sonar' must match the name in Manage Jenkins > Configure System
                withSonarQubeEnv('techie-sonar') {
                    script {
                        // 'sonar-scanner' must match the name in Manage Jenkins > Global Tool Configuration
                        def scannerHome = tool 'sonar-scanner'
                        
                        // Added sonar.projectVersion and removed the single quotes around the project name 
                        // to prevent shell parsing issues
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=techie-horizon -Dsonar.projectName=Techie-Horizon -Dsonar.projectVersion=1.0 -Dsonar.sources=."
                    }
                }
            }
        }
    }
}
