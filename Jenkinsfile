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
                // Compile Python code if needed (optional for Python)
                // sh 'python3 -m py_compile src/*.py'

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
                sh 'python3 -m unittest discover -s src -p "test_*.py"'
            }
        }

        stage('deploy') {
            steps {
                // Ensure virtual environment is activated
                sh '''
                . venv/bin/activate
                nohup python3 src/app.py > app.log 2>&1 &
                '''
            }

            post {
                success {
                    echo 'Application deployed successfully!'
                    echo 'You can access it at: http://localhost:5000'
                }
            }
        }
    }
}
