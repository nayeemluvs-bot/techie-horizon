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
}
