pipeline {
    agent any
    tools {
        maven 'Maven 3.9'  // Use the Maven tool from Jenkins' Global Tool Configuration
    }

    parameters {
        string(name: 'GIT_REPO_URL', defaultValue: 'https://github.com/demuduraviteja/myspringboot-application.git', description: 'Git Repository URL')
        string(name: 'GIT_BRANCH', defaultValue: 'master', description: 'Branch to checkout')
    }

    environment {
        // Define global variables for the repository URL and branch
        GIT_REPO_URL = "${params.GIT_REPO_URL}"
        GIT_BRANCH = "${params.GIT_BRANCH}"
        NEXUS_REPO_URL = 'https://nexus.example.com/repository/maven-releases/'  // Define the Nexus repository URL
        NEXUS_REPO_ID = 'nexus-repo'  // Define the Nexus repository ID
        IMAGE_NAME = 'your-image-name'
        IMAGE_TAG = 'latest'
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = 'your-aws-account-id'
        ECR_REPO_NAME = 'your-ecr-repo-name'
        ECR_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Now you can directly call the shared library function without passing parameters
                    gitCheckout()  // Parameters are automatically available from environment variables
                }
            }
        }
            stage('Maven Build') {
            steps {
                script {
                    // Call the shared library (without passing the mavenHome)
                    mavenBuild()
                }
            }
        }
        stage('Sonar Scan') {
            steps {
                script {
                    sonarScan()
                }
            }
        }
        stage('Sonar Quality gate Check') {
            steps {
                script {
                    qualitygateCheck()
                }
            }
        }
        stage('Upload to Nexus') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USERNAME')]) {
                        nexusUpload()
                    }
                }
            }
    }
    stage('Build Docker Image') {
            steps {
                script {
                    // Call the shared library function to build the Docker image (no parameters needed)
                    dockerBuild()
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Call the shared library function to push the image to ECR (no parameters needed)
                    imageUploadtoECR()
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Call the shared library function to deploy to Kubernetes
                    deployToK8s()  // No need to pass parameters
                }
            }
        }
 }
}
