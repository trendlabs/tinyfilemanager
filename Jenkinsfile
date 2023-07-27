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
                script {
                    app = docker.build("${REGISTRY_URL}/tinyfilemanager/tinyfilemanager")
                }
                echo 'End ---- Build image -----'
            }
        }

        stage('Push image') {
            steps {
                echo 'Start ---- Push image -----'
                script {
                    docker.withRegistry('${REGISTRY_URL}', 'ocp-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
                echo 'End ---- Push image -----'
            }
        }
    }
}
