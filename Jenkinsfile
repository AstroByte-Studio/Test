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
                        sh 'gh release create b7 --title \'Build #7\' ./target/*.jar'
                    }
                }
            }
        }
    }
}
