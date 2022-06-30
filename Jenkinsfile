pipeline {hh

agent any
   
stages{

    stage('Docker Build and Push to dev ecr') {
        when {
            branch "main"
        }
        steps {
            echo "Building phase started"
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/python:latest ." 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/python:latest"
        }
    }

    stage('Updating and deploying k8s components') {
        when {
            branch "main"
        }
        steps {
            sh "kubectl apply -f deployment.yaml"
            
        }
    }
 }
}
