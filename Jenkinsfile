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

        stage("GitHub Release") {
            steps {
                script {
                    def artifactFile = findFiles(glob: 'target/*.jar').first()

                    // Mark the build as successful even if the release fails
                    currentBuild.result = 'SUCCESS'

                    // Publish the release using the GitHub Release Plugin
                    githubRelease(
                        releaseNotes: "Release Notes for v1.0.0",  // Provide release notes
                        tagName: 'v1.0.0',
                        releaseName: 'Release 1.0.0',
                        targetCommitish: 'main',
                        useGitHubCommiters: true,
                        assets: [
                            [
                                pattern: "target/*.jar",
                                // Optionally specify content type
                                contentType: 'application/java-archive'
                            ]
                        ]
                    )
                }
            }
        }
    }
}
