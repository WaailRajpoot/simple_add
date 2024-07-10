pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                // Checkout the code from GitHub
                checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/WaailRajpoot/simple_add.git']]])
            }
        }

        stage('build') {
            steps {
                // Install dependencies if using a requirements.txt file
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'

                // Stash source files for later stages
                stash includes: 'src/*.py', name: 'stashing_build'
            }
        }

        stage('test') {
            steps {
                // Unstash source files for testing
                unstash 'stashing_build'

                // Run unit tests (adjust the command as per your test structure)
                sh '. venv/bin/activate && python3 -m unittest discover -s src -p "test_*.py"'
            }
        }

        stage('deploy') {
            steps {
                script {
                    // Create a shell script to activate the virtual environment and run the Flask app
                    sh '''#!/bin/bash
                    source venv/bin/activate
                    FLASK_APP= simple_add/src/calc.py flask run --host=0.0.0.0 --port=3050 &
                    '''
                }
            }

            post {
                success {
                    echo 'Application deployed successfully!'
                    echo 'You can access it at: http://localhost:3050'
                    echo "I am Waail"
                }
            }
        }
    }
}
