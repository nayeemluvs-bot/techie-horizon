pipeline {
    agent any

    //MANDATORY for the GitHub webhook trigger to work
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Using the Git SCM step to pull from GitHub explicitly
                git branch: 'main',
                    url: 'https://github.com/nayeemluvs-bot/techie-horizon.git'
                echo 'Code checked out successfully!'
            }
        }

        stage('Verify Source') {
            steps {
                sh '''
                    set -eu
                    test -s index.html
                    grep -qi '<!DOCTYPE html>' index.html
                    grep -q '<title>' index.html
                    echo "Source verification passed"
                '''
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

                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=techie-horizon -Dsonar.projectName=Techie-Horizon -Dsonar.projectVersion=1.0 -Dsonar.sources=."
                    }
                }
            }
        }

        stage('Package Website') {
            steps {
                sh '''
                    set -eu
                    rm -f techie-horizon.zip
                    zip techie-horizon.zip index.html
                    unzip -t techie-horizon.zip
                    echo "Website ZIP package created"
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS'
                    )
                ]) {
                    sh '''
                        set +x
                        set -eu
                        curl --fail --show-error --silent \
                            --user "$NEXUS_USER:$NEXUS_PASS" \
                            --upload-file techie-horizon.zip \
                            "http://localhost:8081/repository/techie-horizon-artifacts/techie-horizon/build-${BUILD_NUMBER}/techie-horizon.zip"
                        echo "Artifact uploaded to Nexus successfully"
                    '''
                }
            }
        }
    } // End of Stages

    // --- POST BUILD ACTIONS ---
    post {
        success {
            archiveArtifacts(
                artifacts: 'techie-horizon.zip',
                fingerprint: true
            )
            // Optional: Trigger the next project if needed (uncomment the line below)
            // build job: 'hiringapp-Build_push_update_manifest', wait: false

            echo 'TECHIE HORIZON NEXUS UPLOAD SUCCESSFUL'
        }
        failure {
            echo 'TECHIE HORIZON NEXUS UPLOAD FAILED'
        }
    }
}
