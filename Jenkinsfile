pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh 'echo "MAJOR.MINOR.BUILD: " $MAJOR_VERSION"."$MINOR_VERSION"."$BUILD_NUMBER '
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
    }
}