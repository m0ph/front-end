pipeline {
    environment {
        DOCKER_ID = "m0ph"
        DOCKER_IMAGE = "front-end"
        DOCKER_TAG = "${BUILD_ID}"
    }

agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh '''
                    docker rm -f $DOCKER_IMAGE
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE:DOCKER_TAG .
                    '''
                }
            }
        }
    }
}