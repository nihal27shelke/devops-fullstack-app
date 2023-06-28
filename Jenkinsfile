pipeline {
    agent any
    
    stages {
        stage('Build and Deploy frontend') {
            when {
                changeset "frontend/**"
            }
            steps {
                dir('frontend') {
                    // Build and deploy frontend 
                    sh'echo "Inside Docker Block"'
					sh 'echo "Docker Login : In Progress"'
					sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo'
					sh 'echo "Docker Login : Completed"'
					sh 'echo "Docker Pull : In Progress"'
                    sh 'docker build -t 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo:frontend-latest .'
                    sh 'docker push 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo:frontend-latest'
                    sh 'kubectl apply -f k8s/  -n dev'
                }
            }
        }
        
        stage('Build and Deploy backend') {
            when {
                changeset "backend/**"
            }
            steps {
                dir('backend') {
                    sh'echo "Inside Docker Block"'
					sh 'echo "Docker Login : In Progress"'
					sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo'
					sh 'echo "Docker Login : Completed"'
					sh 'echo "Docker Pull : In Progress"'
                    sh 'docker build -t 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo:backend-latest .'
                    sh 'docker push 836855437023.dkr.ecr.ap-south-1.amazonaws.com/examplecode-ecr-repo:backend-latest'
                    sh 'kubectl apply -f k8s/  -n dev'
                }
            }
        }
    }
    
    post {
        success {
            sh 'echo Pipeline succeeded!'
        }
        
        failure {
            sh 'echo Pipeline failed!'
        }
    }
}

