pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '222634377087' // Replace with your actual AWS Account ID
    }

    stages {

        stage('Login to AWS ECR') {
            steps {
                echo "🔐 Logging into AWS ECR once for all services..."
                script {
                    sh """
                        aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Build & Push Docker Images (excluding cartservice)') {
            steps {
                script {
                    def services = [
                        'adservice',
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

        stage('Build & Push cartservice Docker Image') {
            steps {
                dir('src/cartservice') {
                    echo "🔧 Processing service: cartservice"

                    sh """
                        echo "🚧 Building Docker image for cartservice..."
                        docker build -t cartservice:latest .

                        echo "🔁 Tagging Docker image for ECR..."
                        docker tag cartservice:latest ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/cartservice:latest

                        echo "📤 Pushing Docker image to ECR..."
                        docker push ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/cartservice:latest
                    """
                }
            }
        }
    }
}
