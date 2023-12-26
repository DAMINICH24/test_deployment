pipeline {
    environment { 
    BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
    stages {
    stage('promote') {
when {
    anyOf {
        expression { BRANCH_NAME == "dev"}
        expression { BRANCH_NAME == "preprod"}   
    }
}
steps {
                script {                   
                    try {
                        switch(BRANCH_NAME) {
                          case "dev":
                            gitops.promoteImagesToEnv('uat')
                            gitops.setCommitStatus(GIT_URL, GIT_COMMIT, checkName, 'Promoted to clone', 'SUCCESS')
                          case "preprod":
                            gitops.promoteImagesToEnv('prod')
                            gitops.setCommitStatus(GIT_URL, GIT_COMMIT, checkName, 'Promoted to prod', 'SUCCESS')
                        }
                    } catch (e) {
                        println e
                        gitops.setCommitStatus(GIT_URL, GIT_COMMIT, checkName, 'Promotion failed', 'FAILURE')
                        //Using catchError helps to avoid Jenkins plugin to send error status to Github commit as well as it marks Stage as Failed
                        catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                        error(e.getMessage())
                        }
}
    }
