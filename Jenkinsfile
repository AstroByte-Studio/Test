pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('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')
        MAVEN_HOME = tool 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "${MAVEN_HOME}/bin/mvn clean package"
                }
            }
        }

        stage('Create GitHub Release') {
            steps {
                script {
                    def response = httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_JSON',
                        httpMode: 'POST',
                        requestBody: [
                            tag_name: "${env.BUILD_NUMBER}",
                            name: "Release ${env.BUILD_NUMBER}",
                            body: releaseNotes
                        ],
                        url: "https://api.github.com/repos/AstroByte-Studio/Test/releases",
                        authentication: 'GITHUB_TOKEN'
                    )

                    echo "GitHub Release created. Response: ${response}"
                }
            }
        }

        stage('Upload to GitHub Release') {
            steps {
                script {
                    def artifactFile = findFiles(glob: 'target/*.jar').first()

                    def response = httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_OCTET_STREAM',
                        httpMode: 'POST',
                        requestBody: artifactFile,
                        url: "https://uploads.github.com/repos/AstroByte-Studio/Test/releases/${env.BUILD_NUMBER}/assets?name=${artifactFile.name}",
                        authentication: 'GITHUB_TOKEN'
                    )

                    echo "Artifact uploaded to GitHub Release. Response: ${response}"
                }
            }
        }
    }
}
