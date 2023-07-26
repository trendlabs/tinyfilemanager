pipeline {
    agent any

    options {
        buildDiscarder logRotator( 
            daysToKeepStr: '15', 
            numToKeepStr: '10'
        )
    }

    environment {
        REGISTRY_URL = "image-registry.openshift-image-registry.svc.cluster.local:5000"
        GIT_URL = "https://github.com/trendlabs/tinyfilemanager.git"
    }

    stages {

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace"
                """
            }
        }

        stage('Code Checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: ${GIT_URL}]])
            }
        }

        stage('Build image') {
            steps {
                echo 'Start ---- Build image -----'
                sh """
                    podman login --tls-verify=false -u thanhnq -p \$(oc whoami -t) ${REGISTRY_URL}

                    podman build --tls-verify=false --squash -t ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager -f ./Dockerfile
                """
                echo 'End ---- Build image -----'
            }
        }

        stage('Push image') {
            steps {
                echo 'Start ---- Push image -----'
                sh """
                    podman push --tls-verify=false ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager:latest

                """
                echo 'End ---- Build image -----'
            }
        }
    }
}
