pipeline {
    agent any

    environment {
        GITHUB_CREDENTIALS = credentials('multi-branch-pipeline')
        BUILD_FOLDER = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                              branches: [[name: '*/main']],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [[$class: 'CloneOption', noTags: false, shallow: true, depth: 1, reference: '', referenceRepository: '', submoduleCfg: [], timeout: 120]],
                              submoduleCfg: [],
                              userRemoteConfigs: [[credentialsId: 'multi-branch-pipeline', url: 'https://github.com/waqasyaqoob87/WEATHER_APP.git']]])
                }
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
                    def xamppHtdocs = 'C:\xampp\htdocs\Nasb'
                    def sourceFolder = "${env.WORKSPACE}\\${BUILD_FOLDER}"

                    echo "Copying files from ${sourceFolder} to ${xamppHtdocs}\\"

                    bat "xcopy /s /y \"${sourceFolder}\" \"${xamppHtdocs}\\\""
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
