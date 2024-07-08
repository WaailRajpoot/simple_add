pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myflaskapp"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '55aec472-e876-415a-a546-d0ef907b7cc1', url: 'https://github.com/harsh-singhal7385/simple_add.git']]]) 
            }
        }
    
        stage('Build') {
            steps {
                script {
                    def image = docker.build("$:myflaskapp $:latest")
                }
            }
        }
        
        stage('Test') {
            steps {
                sh 'python3 -m unittest src/test_calc.py'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    
                    sh 'docker-compose up -d' // Start the container in detached mode
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
        success {
            echo "Pipeline succeeded."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
