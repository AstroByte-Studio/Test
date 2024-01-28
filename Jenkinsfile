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
                    git branch: 'main', remote: 'origin', url: 'https://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@github.com/AstroByte-Studio/test.git';
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

        stage("Upload to GitHub Release") {
            steps {
                script {
                    def artifactFile = findFiles(glob: 'target/*.jar').first()

                    def response = httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_OCTET_STREAM',
                        httpMode: 'POST',
                        requestBody: artifactFile,
                        url: "https://uploads.github.com/repos/AstroByte-Studio/test/releases/latest/assets?name=${artifactFile.name}",
                        authentication: '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'
                    )

                    echo "Artifact uploaded to GitHub Release. Response: ${response}"
                }
            }
        }

    }
}
