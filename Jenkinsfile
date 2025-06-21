pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '222634377087' // Replace with your actual AWS Account ID
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
                    ]

                    for (svc in services) {
                        dir("src/${svc}") {
                            echo "🔧 Processing service: ${svc}"

                            sh """
                                echo "Logging into AWS ECR..."
                                aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com

                                echo "🚧 Building Docker image for ${svc}..."
                                docker build -t ${svc}:latest .

                                echo "🔁 Tagging Docker image for ECR..."
                                docker tag ${svc}:latest ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/${svc}:latest

                                echo "📤 Pushing Docker image to ECR..."
                                docker push ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/${svc}:latest
                            """
                        }
                    }
                }
            }
        }
    }
}
