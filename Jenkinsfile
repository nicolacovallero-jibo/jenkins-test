pipeline {  
   agent any 
   stages{
      stage('Checking for mergeability')  {
         steps{
            script{
             if ("${GITHUB_PR_MERGEABLE}" != "true"){
               BODY+="\n\nBranch ${GITHUB_PR_SOURCE_BRANCH} cannot be automatically merged to ${GITHUB_PR_TARGET_BRANCH}, the job has been aborted."
               currentBuild.result = 'ABORTED'
               error("Branch ${GITHUB_PR_SOURCE_BRANCH} cannot be automatically merged to ${GITHUB_PR_TARGET_BRANCH}, the job has been aborted.")
             }
            }
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
