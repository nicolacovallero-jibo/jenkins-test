#!groovy
node {
    stages {
        stage('Test') {
            steps {
                sh 'echo test'
            }
        }
        stage('Removing merged branch') {
            steps {
               sh ' git push origin --delete ${GITHUB_PR_SOURCE_BRANCH}'
            }
        }
    }
}

def setBuildStatus(String message, String state, String context, String sha, String repoUrl) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: repoUrl],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        commitShaSource: [$class: "ManuallyEnteredShaSource", sha: sha ],
        statusBackrefSource: [$class: "ManuallyEnteredBackrefSource", backref: "${BUILD_URL}flowGraphTable/"],
        statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ]); 
}
