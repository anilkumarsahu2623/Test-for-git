pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/anilkumarsahu2623/small-python-app.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh '. venv/bin/activate && pytest --maxfail=1 --disable-warnings -q --tb=short'
            }
        }

        stage('Generate Test Reports') {
            steps {
                sh '. venv/bin/activate && pytest --maxfail=1 --disable-warnings -q --tb=short --junitxml=report.xml'
                archiveArtifacts artifacts: 'report.xml', allowEmptyArchive: true
            }
        }

        stage('Verify Build Logs') {
            steps {
                script {
                    def buildLog = readFile('build.log')
                    echo buildLog
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Tests successful!'
        }
        failure {
            echo 'Build or Tests failed!'
        }
    }
}
