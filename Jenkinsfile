pipeline {
    agent any
    environment {
        registry = "http://192.168.1.55:8595"
        registryCredential = 'dockernexus'
    }

    stages {
        stage('Build') {
            steps {
                withDockerRegistry([
                    credentialsId: "${registryCredential}",
                    url: "${registry}" ]) {
                        sh "docker build . -t ${registry}/hello:v2"
                        sh "docker push ${registry}/hello:v2"

                }
            }
        }
    }
}