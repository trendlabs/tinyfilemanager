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

    
        stage('Clone from git') {
            steps {
               git "${GIT_URL}"
            }
        }

        stage('Build image') {
            steps {
                echo 'Start ---- Build image -----'
                sh """
                    docker login --tls-verify=false -u thanhnq -p \$(oc whoami -t) ${REGISTRY_URL}

                    docker build --tls-verify=false --squash -t ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager -f ./Dockerfile
                """
                echo 'End ---- Build image -----'
            }
        }

        stage('Push image') {
            steps {
                echo 'Start ---- Push image -----'
                sh """
                    docker push --tls-verify=false ${REGISTRY_URL}/tinyfilemanager/tinyfilemanager:latest

                """
                echo 'End ---- Push image -----'
            }
        }
    }
}
