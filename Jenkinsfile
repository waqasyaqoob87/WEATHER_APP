import hudson.FilePath
import jenkins.model.Jenkins

pipeline {
    agent any
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }
 
        stage('Build') {
            steps {
                script {
                    bat 'npm run build'
                }
            }
        }
 
        stage('Copy to Apache DocumentRoot') {
            steps {
                script {
                    copyToApacheDocumentRoot()
                }
            }
        }
    }
 
    post {
        success {
            echo 'Build and deployment successful!'
        }
 
        failure {
            echo 'Build or deployment failed. Check the Jenkins console output for more details.'
        }
    }
}

@NonCPS
def copyToApacheDocumentRoot() {
    def apacheDocumentRoot = 'C:\\xampp\\htdocs'  // Default Apache DocumentRoot
    def distFolder = 'dist'

    // Check if dist folder exists before copying
    def distPath = "${WORKSPACE}\\${distFolder}"
    if (new File(distPath).isDirectory()) {
        echo "Dist folder exists. Copying to Apache DocumentRoot."

        // Use FilePath to copy files
        def workspacePath = new FilePath(new File(WORKSPACE))
        def apacheDocumentRootPath = new FilePath(new File(apacheDocumentRoot))

        // Copy files using FilePath
        workspacePath.copyRecursiveTo("**/${distFolder}/**", apacheDocumentRootPath)

    } else {
        error "Dist folder not found. Skipping copy to Apache DocumentRoot."
    }
}
