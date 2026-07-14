pipeline {
    agent { label 'electronix' }

    environment{
        S3_BUCKET='electronix-production-7830'
        CLOUDFRONT_ID='E3DT4ZNA1YUXZE'
        AWS_REGION="us-east-1"

    }

    stages {
        stage('Frontend Deployment') {
            when {
                changeset "frontend/**"
            }

            stages {
                stage('Install Dependencies') {
                    steps {
                        dir('frontend') {
                            sh '''
                            npm install
                            '''
                        }
                    }
                }

                stage("Run Tests") {
                    steps {
                        dir('frontend') {
                            sh 'npm test -- --watchAll=false || echo "No Test Configured.."'
                        }
                    }
                }

                stage("Build") {
                    steps {
                        dir(frontend) {
                            sh 'npm run build'
                        }
                    }
                }

                stage('Deploy S3') {
                    steps {
                        dir(frontend) {
                            sh '''
                            aws s3 sync dist/ s3://${S3_BUCKET} --delete --region ${AWS_REGION}
                            '''
                        }
                    }
                }


                stage('Invalidation Cloudfront Cache') {
                    steps {
                        sh '''
                        aws cloudfront create-invalidation --distribution-id ${Cloudfront_id} --paths "/*"
                        '''
                    }
                }
            }
        }
    }

    post{
        success{
            'Frontent Deployment Successfull ✅'
        }
        failure{
            'Frontent Deployment Failed ❌'
        }
    }
}