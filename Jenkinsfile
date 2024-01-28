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

                    sh """
                        curl -H "Authorization: token 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" \
                             -H "Content-Type: application/octet-stream" \
                             --data-binary "@${artifactFile}" \
                             "https://uploads.github.com/repos/AstroByte-Studio/test/releases/assets?name=${artifactFile.name}"
                    """
                }
            }
        }
    }
}
