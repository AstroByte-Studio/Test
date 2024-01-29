pipeline {
    agent any

    tools {
        maven 'maven-3.9.5'
        jdk "Java17"
    }

    environment {
        GITHUB_TOKEN_CREDS = credentials('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')
    }

    stages {
        stage("Check out") {
            steps {
                script {
                    // Checkout code with credentials
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/AstroByte-Studio/test.git', credentialsId: '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']]])
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    sh "mvn clean package"
                }
            }
        }

        stage("Set Git Config") {
            steps {
                script {
                    // Set Git user identity
                    sh """
                        git config --global user.email "phoenixgamer989@gmail.com"
                        git config --global user.name "PhoenixGamer339"
                    """
                }
            }
        }

        stage("GitHub Release") {
            steps {
                script {
                    // Use withCredentials to securely handle the GitHub token
                    withCredentials([string(credentialsId: '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', variable: 'GITHUB_TOKEN')]) {
                        // Run gh release create with the securely retrieved token
                        sh "gh release create b7 --title 'Build #7' /target/*.jar"
                    }
                }
            }
        }
    }
}
