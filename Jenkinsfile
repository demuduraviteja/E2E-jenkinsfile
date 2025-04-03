@Library('my-e2e-shared-lib') _

node {
    // Define tools
    tool name: 'Maven 3.9', type: 'hudson.tasks.Maven$MavenInstallation'

    // Parameters
    def GIT_REPO_URL = params.GIT_REPO_URL ?: 'https://github.com/demuduraviteja/myspringboot-application.git'
    def GIT_BRANCH = params.GIT_BRANCH ?: 'master'

    // Environment variables
    env.IMAGE_NAME = 'your-image-name'
    env.IMAGE_TAG = 'latest'
    env.AWS_REGION = 'ap-south-1'
    env.AWS_ACCOUNT_ID = 'your-aws-account-id'
    env.ECR_REPO_NAME = 'your-ecr-repo-name'
    env.ECR_REGISTRY = "${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com"

    try {
        stage('Checkout') {
            gitCheckout()
        }

        stage('Maven Build') {
            mavenBuild()
        }

        stage('Sonar Scan') {
            sonarScan()
        }

        stage('Upload to Nexus') {
            nexusUpload()
        }

        stage('Build Docker Image') {
            dockerBuild()
        }

        stage('Push Docker Image to ECR') {
            imageUploadtoECR()
        }

        stage('Deploy to Kubernetes') {
            deployToK8s()
        }

    } catch (Exception e) {
        echo "Pipeline failed: ${e.getMessage()}"
        currentBuild.result = 'FAILURE'
        throw e
    }
}
