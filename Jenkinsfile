
pipeline {
    agent any
    environment {
        // You can set environment variables here
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }
    tools {
        maven 'Maven 3'  // Define your Maven installation name from Jenkins Global Tool Configuration
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sleep 3
            }
        }
         stage('Test') {
            steps {
                sh 'mvn clean test'
            }
        }
        /*stage('Publish Test Results') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }*/
        stage('Registering build artifact') {
            steps {
                script {
                    echo 'Registering the metadata'
                    def artifactId = registerBuildArtifactMetadata(
                        name: "My New TestApp",
                        version: "2.0.0",
                        type: "docker",
                        url: "http://localhost:1112",
                        digest: "45656064707039346163693937",
                        label: "bug-bash"
                    )
                    echo "Artifact Id is: ${artifactId}"
                    env.ARTIFACT_ID = artifactId
                    sleep 3
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                registerDeployedArtifactMetadata(
                    artifactId: "${env.ARTIFACT_ID}",
                    targetEnvironment: "pre-prod",
                    labels: "CBCI"
                )
            }
        }
        /*stage('Deploy to Preprod') {
            steps {
                echo 'Deploying...'
                registerDeployedArtifactMetadata(
                    artifactId: "${env.ARTIFACT_ID}",
                    targetEnvironment: "pre-prod",
                    labels: "pre-prod"
                )
            }
        }
        stage('Deploy to QA') {
            steps {
                echo 'Deploying...'
                registerDeployedArtifactMetadata(
                    artifactId: "${env.ARTIFACT_ID}",
                    targetEnvironment: "qa",
                    labels: "qa"
                )
            }
        }*/
    }
}
