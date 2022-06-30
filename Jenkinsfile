pipeline {

agent any
   
stages{
    // Code quality checks
    // stage('Run quality checks') {
    //     when {
    //         branch 'master'
    //     }
    //     environment {
    //         scannerHome = tool 'SonarQubeScanner'
    //     }
    //     steps {
    //         withSonarQubeEnv('sonar') {
    //             sh '${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=epw-notification-service -Dsonar.sources=. -Dsonar.host.url=https://inspector.sa-labs.info'
    //         }
    //     }
    // }
    
     // Code quality gate checks
    // stage ("SonarQube Quality Gate") {
/*        when {
            allOf {
                branch 'master'   
            }
        }  */
    //  steps {
    //      script {
    //          def qualitygate = waitForQualityGate() 
    //          if (qualitygate.status != "OK") {
    //              error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
    //             }
    //         }
    //     }
    // }
    // Create Container and push to Container registry
    stage('Docker Build and Push to dev ecr') {
        when {
            branch 'main'
        }
        steps {
            echo "Building phase started"
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 481195286011.dkr.ecr.us-east-1.amazonaws.com"
            sh "docker build -t 481195286011.dkr.ecr.us-east-1.amazonaws.com/python:latest" 
            sh "docker push 481195286011.dkr.ecr.us-east-1.amazonaws.com/python:latest"
        }
    }

