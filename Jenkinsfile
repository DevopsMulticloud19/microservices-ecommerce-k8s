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
                try {
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
                } catch (Exception e) {
                    echo "⚠️ Failed to process ${svc}: ${e.getMessage()}"
                    // Continue to next service
                }
            }
        }
    }
}
