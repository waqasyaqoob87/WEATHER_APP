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

        stage('Copy to XAMPP') {
            steps {
                script {
                    def xamppHtdocs = 'C:\\xampp\\htdocs'
                    def distFolder = 'dist'

                    // Check if dist folder exists before copying
                    def distPath = "${WORKSPACE}\\${distFolder}"
                    if (dirExists(distPath)) {
                        echo "Dist folder exists. Copying to XAMPP."
                        bat "xcopy /s /y ${distPath} ${xamppHtdocs}\\"
                    } else {
                        error "Dist folder not found. Skipping copy to XAMPP."
                    }
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
