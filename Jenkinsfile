pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME == "develop" ? "true" : "false"}"
        NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        VERSION = '1.0.0'
        DOMAIN = 'localhost'
        REGISTRY = 'suayb/golang-echo-graphql-example'
        REGISTRY_CREDENTIAL = 'dockerhub-creds'
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'build.yaml'
        }
    }
    stages {
        stage('Docker Build') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                container('docker') {
                    sh "docker build -t ${REGISTRY}:${VERSION} ."
                }
            }
        }
        stage('Docker Publish') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                container('docker') {
                    withDockerRegistry([credentialsId: "${REGISTRY_CREDENTIAL}", url: ""]) {
                        sh "docker push ${REGISTRY}:${VERSION}"
                    }
                }
            }
        }
    }
}