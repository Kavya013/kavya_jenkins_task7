pipeline { 
    agent any 

    tools { 
        dockerTool 'Docker'  
    }

    environment { 
        PYTHON_HOME = "C:\\Users\\prabh\\AppData\\Local\\Programs\\Python\\Python311" 
        DOCKER_HOME = "C:\\Program Files\\Docker\\Docker\\resources\\bin"
        PATH = "${PYTHON_HOME};${PYTHON_HOME}\\Scripts;${DOCKER_HOME};C:\\Windows\\System32;C:\\Windows"  
        DOCKER_IMAGE = "flask-app"
    } 

    stages { 
        stage('Initialize') { 
            steps { 
                script {
                    echo "Initializing Pipeline..."
                    bat 'python --version'  
                    bat 'docker --version'  
                }
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Kavya013/kavya_jenkins_task7'
            }
        }

        stage('Install Dependencies') { 
            steps { 
                script {
                    echo "Installing Python dependencies..."
                    bat 'pip install -r requirements.txt'  
                }
            }
        }

        stage('Build Docker Image') { 
            when { expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' } }
            steps { 
                script {
                    echo "Building Docker Image..."
                    bat 'docker build -t %DOCKER_IMAGE% .'  
                }
            }
        }

        stage('Deploy with Docker') { 
            when { expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' } }
            steps { 
                script {
                    echo "Deploying application using Docker..."
                    bat 'docker stop flask-container || echo Container not running'  
                    bat 'docker rm flask-container || echo Container not found'   
                    bat 'docker run -d -p 5000:5000 --name flask-container %DOCKER_IMAGE%'  
                    echo "Application is accessible at http://localhost:5000"
                }
            }
        }
    }
        post {
        success {
            echo 'Deployment Successful! Flask App is running on port 5000'
        }
        failure {
            echo 'Deployment Failed. Check logs!'
        }
    }
}
