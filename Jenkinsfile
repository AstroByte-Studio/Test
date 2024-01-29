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

                /*stage("Set Git Config") {
                    steps {
                        script {
                            // Set Git user identity
                            sh """
                                git config --global user.email "phoenixgamer989@gmail.com"
                                git config --global user.name "PhoenixGamer339"
                            """
                        }
                    }
                }*/

        stage("GitHub Release") {
            steps {
                script {
                    withCredentials([string(credentialsId: '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', variable: 'GH_TOKEN')]) {
                        sh 'gh release create b7 --title \'Build #7\' ./target/*.jar'
                    }
                    
                }
            }
        }
    }
}
