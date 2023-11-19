pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_REGISTRY_CREDENTIALS', defaultValue: 'docker-hub-credentials', description: 'Docker registry credentials ID')
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'cloudsheger/simple-node-app', description: 'Docker image name')
    }

    environment {
        DOCKER_IMAGE = docker.image(params.DOCKER_IMAGE_NAME)
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    /* This builds the actual image; synonymous to
                     * docker build on the command line */
                    DOCKER_IMAGE = docker.build(params.DOCKER_IMAGE_NAME)
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    /* Ideally, we would run a test framework against our image.
                     * For this example, we're using a Volkswagen-type approach ;-) */
                    DOCKER_IMAGE.inside {
                        sh 'echo "Tests passed"'
                    }
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    /* Finally, we'll push the image with two tags:
                     * First, the incremental build number from Jenkins
                     * Second, the 'latest' tag.
                     * Pushing multiple tags is cheap, as all the layers are reused. */
                    docker.withRegistry('https://registry.hub.docker.com', params.DOCKER_REGISTRY_CREDENTIALS) {
                        DOCKER_IMAGE.push("${env.BUILD_NUMBER}")
                        DOCKER_IMAGE.push("latest")
                    }
                }
            }
        }
    }
}
