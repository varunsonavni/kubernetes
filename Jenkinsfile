pipeline {

agent any
   
stages{

    stage('Docker Build and Push to dev ecr') {
        when {
            branch "main"
        }
        // steps {
        //     echo "Building phase started."
        //     nodejs('NodeJS-16.0.0') {
               
        //        sh 'pm2 --version'
        //     }
            echo "Building phase started."
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:prod ." 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/sample-python-frontend:prod"
            
        }
    }

//     stage('Updating and deploying k8s components') {
//         when {
//             branch "main"
//         }
//         steps {
//             sh "kubectl apply -f deployment.yaml"
//             sh "kubectl rollout restart deployment -n stage epw-notification-deployment"
            
//         }
//     }
 }
}
