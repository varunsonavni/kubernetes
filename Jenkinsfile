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
            branch 'master'
        }
        steps {
            echo "Building phase started"
            sh "aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com"
            sh "docker build -t epw-notification-service ."
            sh "docker tag epw-notification-service:latest 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com/epw-notification-service:latest"
            sh "docker push 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com/epw-notification-service:latest"
        }
    }

    // Updating cluster components
    stage('Updating and deploying k8s components') {
        when {
            branch 'master'
        }
        steps {
            sh "kubectl apply -f deployment.yaml"
            sh "kubectl rollout restart deployment -n development epw-notification-deployment"
        }
    }
    // Create Container and push to Container registry
    stage('Docker Build and Push to stage ecr') {
        when {
            branch 'stage'
        }
        steps {
            echo "Building phase started"
            sh "aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com"
            sh "docker build -t epw-notification-service ."
            sh "docker tag epw-notification-service:latest 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com/epw-notification-service:stage"
            sh "docker push 274844568369.dkr.ecr.ap-southeast-2.amazonaws.com/epw-notification-service:stage"
        }
    }

    // Updating cluster components
    stage('Updating and deploying stage components') {
        when {
            branch 'stage'
        }
        steps {
            sh "kubectl apply -f stage-deploy.yaml"
            sh "kubectl rollout restart deployment -n stage epw-notification-deployment"
        }
    }
}
post {
        always {
            cleanWs()
            print "All stages finished running"
        }
        success {
            print "Job finished successfully"
            emailext body: """Hi, \nThe pipeline at Jenkins finished successfully. Pleas go over to the Jenkins and check it out.\n<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>\nThanks!""", subject: "The pipeline Job ${env.JOB_NAME} : has ${currentBuild.currentResult}", to: 'jayesh.desai@solutionanalysts.com', mimeType: 'text/html', replyTo: '$DEFAULT_REPLYTO'
        }
        unstable {
            print "Job finished successfully"
            emailext body: """Hi, \nThe pipeline at Jenkins finished unstable. Pleas go over to the Jenkins and check it out.\n<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>\nThanks!""", subject: "The pipeline Job ${env.JOB_NAME} : has ${currentBuild.currentResult}", to: 'jayesh.desai@solutionanalysts.com', mimeType: 'text/html', replyTo: '$DEFAULT_REPLYTO'
        }
        failure {
            print "Job failed - Fix me"
            emailext body: """Hi, \nThe pipeline at Jenkins has been failed. Pleas go over to the Jenkins and check it out.\n<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>\nThanks!""", subject: "The pipeline Job ${env.JOB_NAME} : has ${currentBuild.currentResult}", to: 'jayesh.desai@solutionanalysts.com', mimeType: 'text/html', replyTo: '$DEFAULT_REPLYTO'
        }                
    }
}
