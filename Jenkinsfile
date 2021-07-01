node {
    //VELOCITY_APP_NAME must match your Velocity pipeline application name
    def GITHUB_REPO_URL="https://github.com/oznbgdemo/demo-db"
    def GITHUB_REPO="https://github.com/oznbgdemo/demo-db"
    def GITHUB_BRANCH="main"    
    def VELOCITY_APP_NAME="GIT-DB"
    def GIT_COMMIT
    def VELOCITY_ENV_ID_INPUT="64fa7beb-4deb-4177-84a0-c018a32bff8e"
    def VELOCITY_ENV_ID_DEV="def86393-cbbf-40b2-a97a-fb08c7cf64b5"
    def VELOCITY_ENV_ID_QA="97002b7a-9de3-4c05-a233-a13669e16438"
    def VELOCITY_ENV_ID_PROD="e4a162e6-badb-4cf3-8600-268e388b653e"
    def VERSION_NUMBER="1.0"    
    
    stage('Build') {
            sh 'echo "Hello World"'
            sh 'echo "MAJOR.MINOR.BUILD: " $MAJOR_VERSION"."$MINOR_VERSION"."$BUILD_NUMBER '
            sh '''
                echo "Multiline shell steps works too"
                ls -lah
            '''
            GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
            echo "GIT_COMMIT=${GIT_COMMIT}"
            currentBuild.displayName = "${MAJOR_VERSION}.${MINOR_VERSION}.${BUILD_NUMBER}"
            echo "Building ${VELOCITY_APP_NAME} (Build:${currentBuild.displayName}, GIT_COMMIT:${GIT_COMMIT})"
            step($class: 'UploadBuild', 
               tenantId: "5ade13625558f2c6688d15ce",
               revision: "${GIT_COMMIT}",
               appName: "${VELOCITY_APP_NAME}",
               versionName:"${currentBuild.displayName}",
               requestor: "admin", id: "${currentBuild.displayName}"
            )
    }
    stage('Build2') {
	    sh 'do something in Build2'
	    post {
                success {
                    publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS"
                }
                failure {
                    publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL"
                }
            }
    }
    stage('Test') {
            echo 'Testing..'
    }
   stage ("Deploy to DEV") {
    sleep 10
    step([$class: 'UploadDeployment',
          tenantId: "5ade13625558f2c6688d15ce",
          versionName: "${currentBuild.displayName}",
          versionExtId: "${currentBuild.displayName}",
          type: 'Jenkins',
          environmentId: "${VELOCITY_ENV_ID_DEV}",
          environmentName: 'DEV',
          appName: "${VELOCITY_APP_NAME}",
          description: '[Description ex: Terraform Deployment]',
          initiator: "admin",
		  result: 'true'
      ])
   }    
    stage('Send Metrics') {
        echo "Building ${VELOCITY_APP_NAME} (Build:${currentBuild.displayName}, GIT_COMMIT:${GIT_COMMIT})" 
        step($class: 'UploadBuild',
            tenantId: "5ade13625558f2c6688d15ce",
            revision: "${GIT_COMMIT}",
            appName: "${VELOCITY_APP_NAME}",
            versionName:"${currentBuild.displayName}",
            requestor: "admin", id: "${currentBuild.displayName}")
    }
    stage('Deploy') {
            echo 'Deploying....'
    }
}
