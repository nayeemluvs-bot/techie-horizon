pipeline {
    agent any

    // This line is MANDATORY for the "GitHub hook trigger" to work
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
                // 'SonarQube' must exactly match the Name you gave in
                // Manage Jenkins -> Configure System -> SonarQube servers
                withSonarQubeEnv('techie-sonar') {
                    // Inside this block, the SONAR_TOKEN is automatically injected.
                    // Now you run your scanner command.
		    def mvnHome = tool 'Maven-3.10.1' // Use the name you gave in Global Tools
		    sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.projectKey=techie-horizon -Dsonar.projectName='Techie Horizon'"	
                }
            }
        }
    } // <--- THIS closing brace was moved down to close the "stages" block
} // <--- This closes the "pipeline" block
