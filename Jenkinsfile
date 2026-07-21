pipeline {
    agent { label 'build-agent-1' }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/laxmanluckymankala631/coyote-logistics-project.git'
            }
        }
        stage('Build with Maven') {
            steps {
                dir('coyote-app') {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Upload Artifact to S3 (LocalStack)') {
            steps {
                dir('coyote-app') {
                    sh '''
                    aws --endpoint-url=http://host.docker.internal:4566 \
                        s3 cp target/*.war \
                        s3://coyote-build-artifacts/coyote-app-${BUILD_NUMBER}.war
                    '''
                }
            }
        }
        stage('Deploy Notification') {
            steps {
                echo "Build ${BUILD_NUMBER} uploaded to S3 - ready for Tomcat deployment"
            }
        }
    }
}
