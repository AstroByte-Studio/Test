pipeline {
    agent any

    tools {
        maven 'maven-3.9.5'
        jdk "Java17"
    }

    stages {
        stage("Check out") {
            steps {
                script {
                    git branch: 'main', credentialsId: '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', url: 'https://github.com/AstroByte-Studio/test.git'
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

        stage("GitHub Release") {
            steps {
                script {
                    // Use withCredentials to securely handle the GitHub token
                    withCredentials([string(credentialsId: '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', variable: 'GH_TOKEN')]) {
                        // Set the GITHUB_TOKEN environment variable explicitly
                        env.GITHUB_TOKEN = GH_TOKEN

                        // Run gh release create with the securely retrieved token
                        sh 'gh release create b7 --title \'Build #7\' ./target/*.jar'
                    }
                }
            }
        }
    }
}
