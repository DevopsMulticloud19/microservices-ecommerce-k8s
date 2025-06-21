pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '222634377087' // Replace with your real AWS Account ID
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
                        def buildPath = (svc == 'cartservice') ? 'src/cartservice/src' : "src/${svc}"

                        dir(buildPath) {
                            echo "🔧 Building and pushing image for ${svc}"

                            sh """
                                set -e
                                set -x

                                echo "🔐 Logging into AWS ECR"
                                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com

                                echo "🛠️ Building Docker image for ${svc}"
                                docker build -t ${svc}:latest .

                                echo "🏷️ Tagging Docker image"
                                docker tag ${svc}:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/${svc}:latest

                                echo "📤 Pushing Docker image to ECR"
                                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/${svc}:latest
                            """
                        }
                    }
                }
            }
        }
    }
}
