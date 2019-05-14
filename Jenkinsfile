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

    post {
        failure {
            // mail body: 'failure body', from: 'lvbo09@163.com', subject: 'build status', to: 'lvbo09@163.com'

            emailext body:
                """<p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER}\'</b></p>
                   <p>View console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"</p>
                   <p><i>(Build log is attached.)</i></p>""",
                compressLog: true,
                attachLog: true,
                recipientProviders: [culprits(), developers(), requestor(), brokenBuildSuspects()],
                replyTo: 'lvbo09@163.com',
                subject: "Status: ${currentBuild.result?:'SUCCESS'} - Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'",
                to: "lvbo09@163.com"
        }
    }

}