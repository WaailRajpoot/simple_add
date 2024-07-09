pipeline {
    agent any

  environment {
        DOCKER_IMAGE = 'your_docker_image_name'
        CONTAINER_NAME = 'your_container_name'
    }
    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '55aec472-e876-415a-a546-d0ef907b7cc1', url: 'https://github.com/harsh-singhal7385/simple_add.git']]])
            }
        }

        stage('build') {
            steps {
                git branch: 'main', credentialsId: '55aec472-e876-415a-a546-d0ef907b7cc1', url: 'https://github.com/harsh-singhal7385/simple_add.git'
                sh 'python3 -m py_compile src/add2vals.py src/calc.py'
                stash includes: 'src/*.py*', name: 'stashing_build'
            }
        }

        stage('test') {
            steps {
                sh 'python3 -m unittest src/test_calc.py'
            }
        }

        stage('deploy') {
            steps {
               ipt {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."

                    // Run Docker container in detached mode
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

