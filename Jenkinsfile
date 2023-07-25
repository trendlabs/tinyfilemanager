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
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/master']], 
                    userRemoteConfigs: [[url: ${GIT_URL}]]
                ])
            }
        }

        stage('Build image') {
            echo '========== START podman login and build ==============='
            sh """
                podman login --tls-verify=false -u thanhnq -p \$(oc whoami -t) ${REGISTRY_URL}

                podman build --tls-verify=false --squash -t ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager -f ./Dockerfile
            """
            echo '========== END podman login and build ==============='
        }

        stage('Push image') {
            echo '========== START podman push image to registry ==============='
            sh """
                podman push --tls-verify=false ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager:latest

            """
            echo '========== END podman push image to registry ==============='
        }

    }
}
