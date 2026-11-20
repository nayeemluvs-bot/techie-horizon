pipeline {
    agent any
    
    //  Test THIS LINE IS MANDATORY for the "GitHub hook trigger" to work
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
    }

    // --- SONARQUBE STAGE ---
    stage('SonarQube Analysis') {
        steps {
            // 'SonarQube' must exactly match the Name you gave in 
            // Manage Jenkins -> Configure System -> SonarQube servers
            withSonarQubeEnv('SonarQube') { 
                    
                // Inside this block, the SONAR_TOKEN is automatically injected.
                // Now you run your scanner command.
                sh 'mvn sonar:sonar -Dsonar.projectKey=techie-horizon -Dsonar.projectName="Techie Horizon"'
                    
                // OR if you are using the Sonar Scanner CLI instead of Maven:
                // sh 'sonar-scanner'
             }
         }
     }
  
}
