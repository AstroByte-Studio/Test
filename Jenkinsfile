pipeline {
    agent any

    tools {
        maven 'maven-3.9.5'
        jdk "Java17"
    }

    environment {
        GITHUB_TOKEN_CREDENTIAL_ID = 'your-github-token-credential-id'
        TAG_NAME = 'v1.0.0'
        RELEASE_NAME = 'Release 1.0.0'
    }

    stages {
        stage("Check out") {
            steps {
                script {
                    git branch: 'main', credentialsId: 'your-git-credentials-id', url: 'https://github.com/AstroByte-Studio/test.git'
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
                    def releaseNotes = "Release Notes for ${TAG_NAME}"

                    // Retrieve GitHub token from Jenkins credentials
                    def githubToken = credentials(GITHUB_TOKEN_CREDENTIAL_ID)

                    // Create a GitHub release using Git and cURL
                    sh """
                        git tag -a ${TAG_NAME} -m "${releaseNotes}"
                        git push origin ${TAG_NAME}

                        curl -H "Authorization: token ${githubToken}" \
                             -H "Content-Type: application/json" \
                             --data '{
                                "tag_name": "${TAG_NAME}",
                                "target_commitish": "main",
                                "name": "${RELEASE_NAME}",
                                "body": "${releaseNotes}",
                                "draft": false,
                                "prerelease": false
                             }' \
                             "https://api.github.com/repos/AstroByte-Studio/test/releases"

                        curl -H "Authorization: token ${githubToken}" \
                             -H "Content-Type: application/octet-stream" \
                             --data-binary "@${artifactFile}" \
                             "https://uploads.github.com/repos/AstroByte-Studio/test/releases/latest/assets?name=${artifactFile.name}"
                    """
                }
            }
        }
    }
}
