pipeline {
    agent any
    environment {
        GIT_URL = "git@github.com:fajritsaniy/jenkins-file-sample.git"
        BRANCH = "master"
        ROBOT = "/home/user/.local/bin/robot"
        CHANNEL = "#jenkins"
        IMAGE = "my-robot-test"
        CONTAINER = "my-robot-test-app"
        DOCKER_APP = "/usr/bin/docker"
    }
    stages{
        // For Docker Clean up
        stage("Cleaning Up") {
            steps {
                echo "Cleaning up"
                sh "sudo ${DOCKER_APP} rm -f ${CONTAINER} || true"
            }
        }
        stage("Clone") {
            steps {
                echo "Clone"
                git branch: "${BRANCH}", url: "${GIT_URL}"
            }
        }
        stage("Build") {
            steps {
                echo "Build"
                sh "sudo ${DOCKER_APP} build -t ${IMAGE} ."
            }
        }
        stage("Run"){
            steps {
                echo "Run Test"
                sh "sudo ${DOCKER_APP} run --rm ${IMAGE}"
            }
        }
    }
    post {
        always {
            echo "This is always run!"
        }
        success {
            echo "Your build is deployed successfully!"
            slackSend(channel: "${CHANNEL}", message: "Your build is deployed successfully!")
        }
        failure {
            echo "Your build is failed!"
            slackSend(channel: "${CHANNEL}", message: "Your build is failed!")
        }
    }
}
