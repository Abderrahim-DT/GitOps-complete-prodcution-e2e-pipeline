pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "complete-production-e2e-deployment"
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                clearWs()
            }
        }
        stage('Checkout from SCM') {
            steps {
                git branch: 'master', credentialsId: 'github-pat', url: "https://github.com/Abderrahim-DT/gitops-complete-production-e2e-deployment"
            }
        }
        stage('Update the deployment tags') {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage('Push the changed deployment file to GitHub') {
            steps {
                sh """
                    git config --global user.name "Abderrahim-DT"
                    git config --global user.email "Abderrahimbenaissa15@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update deployment tag to ${IMAGE_TAG}"
                    git push origin master
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Abderrahim-DT/gitops-complete-production-e2e-deployment master"

                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}