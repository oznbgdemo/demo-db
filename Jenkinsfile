node {
    //VELOCITY_APP_NAME must match your Velocity pipeline application name
    def VELOCITY_APP_NAME="DB"
    def GIT_COMMIT

    stage('Build') {
            sh 'echo "Hello World"'
            sh 'echo "MAJOR.MINOR.BUILD: " $MAJOR_VERSION"."$MINOR_VERSION"."$BUILD_NUMBER '
            sh '''
                echo "Multiline shell steps works too"
                ls -lah
            '''
            GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
            echo "GIT_COMMIT=${GIT_COMMIT}"
    }
    stage('Test') {
            echo 'Testing..'
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
