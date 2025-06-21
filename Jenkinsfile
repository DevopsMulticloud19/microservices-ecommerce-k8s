pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '222634377087'
    }

    stages {
        stage('Build & Push Docker Images to ECR') {
            steps {
                script {
                    def services = [
                        'adservice',
                        'cartservice',
                        'checkoutservice',
                        'currencyservice',
                        'emailservice',
                        'frontend',
                        'paymentservice',
                        'productcatalogservice',
                        'recommendationservice',
                        'shippingservice'
                        'loadgenerator'
                    ]

                    for (service in services) {
                        echo "Checking ECR for image: ${service}:latest"
                        def imageExists = sh(
                            script: """
                            aws ecr describe-images \
                              --repository-name ${service} \
                              --image-ids imageTag=latest \
                              --region ${env.AWS_REGION} >/dev/null 2>&1
                            """,
                            returnStatus: true
                        ) == 0

                        if (imageExists) {
                            echo "Image ${service}:latest already exists in ECR. Skipping build & push."
                        } else {
                            echo "Image ${service}:latest not found. Building & pushing..."

                            sh """
                            docker build -t ${service}:latest src/${service}
                            docker tag ${service}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${service}:latest
                            docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${service}:latest
                            """
                        }
                    }
                }
            }
        }
    }
}
