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
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG .
                    '''
                }
            }
        }
        stage('Run and test') {
            steps {
                sh '''
                docker network create $BUILD_TAG
            	docker run -d --name front-end --rm --network $BUILD_TAG $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
                docker run -d --name curl --rm --network $BUILD_TAG curlimages/curl:latest -L -v http://front-end:8079
                docker stop front-end
                docker network rm $BUILD_TAG
                '''
            }
        }
        stage('Push') {
            environment {
            DOCKER_PASS = credentials("DOCKER_HUB_PASS")
            }
            steps {
                sh '''
                docker image tag $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG $DOCKER_ID/$DOCKER_IMAGE:latest
                docker login -u $DOCKER_ID -p $DOCKER_PASS
                docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
                docker push $DOCKER_ID/$DOCKER_IMAGE:latest
                '''
            }
        }
    }
}