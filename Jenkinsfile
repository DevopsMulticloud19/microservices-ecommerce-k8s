pipeline {
    agent any
    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '222634377087' // 🔁 Replace with your real AWS account ID
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
                            sh """
                            echo "Building and Pushing ${svc}"
                            
<<<<<<< HEAD
                            aws ecr get-login-password --region "$AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com"
                            
                        
=======
                            aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com"
                            
                            docker build -t ${svc}:latest .
>>>>>>> refs/remotes/origin/main
                            docker tag ${svc}:latest "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/${svc}:latest"
                            docker push "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/${svc}:latest"
                            """
                        }
                    }
                }
            }
        }
    }
}


