pipeline {

agent any
   
stages{

    stage('Docker Build and Push to main ecr') {
        when {
            branch "main"
        }
        steps {
            echo "Building phase started."
        //     nodejs('NodeJS-16.0.0') {
               
        //        sh 'pm2 --version'
        //     }
        
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:prod ." 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:prod"
            
        }
    }

    stage('Updating and deploying k8s components to main') {
        when {
            branch "main"
        }
        steps {
            sh "kubectl apply -f deployment.yaml -n prod"
            sh "kubectl rollout restart deployment -n prod python-app-frontend"
            
        }
    }

    stage('Docker Build and Push to stage ecr') {
        when {
            branch "stage"
        }
        steps {
            echo "Building phase started."
        //     nodejs('NodeJS-16.0.0') {
               
        //        sh 'pm2 --version'
        //     }
        
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:stage ." 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:stage"
            
        }
    }

//     stage('Updating and deploying k8s components to stage') {
//         when {
//             branch "stage"
//         }
//         steps {
//             sh "kubectl apply -f deployment.yaml"
//             sh "kubectl rollout restart deployment -n stage epw-notification-deployment"
            
//         }
//     }

    stage('Docker Build and Push to dev ecr') {
        when {
            branch "dev"
        }
        steps {
            echo "Building phase started."
        //     nodejs('NodeJS-16.0.0') {
               
        //        sh 'pm2 --version'
        //     }
        
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:dev ." 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:dev"
            
        }
    }

//     stage('Updating and deploying k8s components to dev') {
//         when {
//             branch "dev"
//         }
//         steps {
//             sh "kubectl apply -f deployment.yaml"
//             sh "kubectl rollout restart deployment -n stage epw-notification-deployment"
            
//         }
//     }
 }
}
