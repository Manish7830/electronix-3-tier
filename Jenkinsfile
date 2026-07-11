pipeline {
    agent { label 'electronix' }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Electronix'
            }
        }

        stage('Setup') {
            steps {
                echo 'Electronix Setup is working'
            }
        }
    }

    post {
        success {
            mail(
                to: 'ms2500287@gmail.com',
                subject: "SUCCESS: ${env.JOB_NAME}",
                body: "Build Successful\n${env.BUILD_URL}"
            )
        }

        failure {
            mail(
                to: 'ms2500287@gmail.com',
                subject: "FAILED: ${env.JOB_NAME}",
                body: "Build Failed\n${env.BUILD_URL}"
            )
        }
    }
}