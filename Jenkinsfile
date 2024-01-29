pipeline {
    agent any

    tools {
        maven 'maven-3.9.5'
        jdk "Java17"
    }

    environment {
        GITHUB_TOKEN = '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'
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
                    sh 'echo $GITHUB_TOKEN > mytoken.txt'
                    sh 'gh auth login --with-token < mytoken.txt'
                    sh 'gh release create b7 --title \'Build #7' + '\' /target/*.jar'
                }
            }
        }
    }
}
