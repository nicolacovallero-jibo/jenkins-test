#!groovy
node {     
   checkout scm
   def url = sh(returnStdout: true, script: 'git config remote.origin.url').trim()

   sleep 10

   setBuildStatus("Complete","SUCCESS","${env.JOB_BASE_NAME}","${env.GITHUB_PR_HEAD_SHA}",url) 

   sleep 10 

   setBuildStatus("Complete","FAILURE","${env.JOB_BASE_NAME}","${env.GITHUB_PR_HEAD_SHA}",url) 


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
           
        
    
    
 
