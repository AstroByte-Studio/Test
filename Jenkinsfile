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
                    sh 'brew install gh'
                    withCredentials([string(credentialsId: 'jenkins', variable: 'GH_TOKEN')]) {
                        sh 'gh release create b3 --title \'Build #3' + '\' /target/*.jar'
                    }
                }
            }
        }
    }
}
