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
                    def tagName = 'v1.0.0'
                    def releaseName = 'Release 1.0.0'
                    def releaseNotes = "Release Notes for ${tagName}"

                    def artifactFile = findFiles(glob: 'target/*.jar').first()

                    // Create a GitHub release using Git and cURL
                    sh """
                        git tag -a ${tagName} -m "${releaseNotes}"
                        git push origin ${tagName}

                        curl -H "Authorization: token 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" \
                             -H "Content-Type: application/json" \
                             --data '{
                                "tag_name": "${tagName}",
                                "target_commitish": "main",
                                "name": "${releaseName}",
                                "body": "${releaseNotes}",
                                "draft": false,
                                "prerelease": false
                             }' \
                             "https://api.github.com/repos/AstroByte-Studio/test/releases"

                        curl -H "Authorization: token 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" \
                             -H "Content-Type: application/octet-stream" \
                             --data-binary "@${artifactFile}" \
                             "https://uploads.github.com/repos/AstroByte-Studio/test/releases/latest/assets?name=${artifactFile.name}"
                    """
                }
            }
        }
    }
}
