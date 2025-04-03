@Library('my-e2e-shared-lib') _

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
        stage('Upload to Nexus') {
            steps {
                script {
                    nexusUpload()
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
                    imageUploadtoDockerHub()
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