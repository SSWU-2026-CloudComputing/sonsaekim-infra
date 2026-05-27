pipeline {
    agent any

    environment {
        CREDENTIALS_ID = '850d941b-7a1b-479d-8e1d-6b0d40b89d68'
        PROJECT_ID     = 'sonsaekim-ai'
        CLUSTER_NAME   = 'k8s'
        LOCATION       = 'asia-northeast3-a'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Deploy Infra') {
            steps {
                script {
                    def manifests = [
                        'k8s/ingress/ingress.yaml',
                        'k8s/rabbitmq/deployment.yaml',
                        'k8s/rabbitmq/service.yaml',
                        'k8s/redis/deployment.yaml',
                        'k8s/redis/service.yaml'
                    ]
                    manifests.each { manifest ->
                        step([
                            $class: 'KubernetesEngineBuilder',
                            projectId: env.PROJECT_ID,
                            clusterName: env.CLUSTER_NAME,
                            location: env.LOCATION,
                            manifestPattern: manifest,
                            credentialsId: env.CREDENTIALS_ID,
                            verifyDeployments: false
                        ])
                    }
                }
            }
        }
    }

    post {
        success { echo "SUCCESS" }
        failure { echo "FAILED" }
    }
}
