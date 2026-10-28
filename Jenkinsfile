
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ramalingam-2006/jenkins-kubernetes-set12.git'
            }
        }

        stage('Test Application') {
            steps {
                bat 'if not exist app\\index.html exit /b 1'
                bat 'findstr /C:"Set 12" app\\index.html'
                bat 'if not exist Dockerfile exit /b 1'
                bat 'if not exist deployment.yaml exit /b 1'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t ramalingam2006/jenkins-kubernetes-set12:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds-set12',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    bat 'echo %DOCKER_TOKEN%| docker login -u "%DOCKER_USER%" --password-stdin'
                    bat 'docker push ramalingam2006/jenkins-kubernetes-set12:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                "C:\\Users\\akkin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" --kubeconfig="C:\\Users\\akkin\\Downloads\\jenkins\\kubeconfig.yaml" apply -f deployment.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '''
                "C:\\Users\\akkin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" --kubeconfig="C:\\Users\\akkin\\Downloads\\jenkins\\kubeconfig.yaml" rollout status deployment/cicd-web --timeout=180s
                "C:\\Users\\akkin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" --kubeconfig="C:\\Users\\akkin\\Downloads\\jenkins\\kubeconfig.yaml" get deployment cicd-web
                '''
            }
        }
    }
}
