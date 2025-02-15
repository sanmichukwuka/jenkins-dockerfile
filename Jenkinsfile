pipeline{
    agent any
    
    parameters {
        string(name: 'REPO_NAME', defaultValue: 'ubuntunginx', description: 'Provide repository name to be pushed ')
        string(name: 'ECR_NAME', defaultValue: 'https://681478331750.dkr.ecr.us-east-1.amazonaws.com', description: 'Provide ECR URL')
    }
    
    options {
        skipStagesAfterUnstable()
    }
   
    stages {
        stage('clone repo') {
            steps {
                script {
                     checkout scm
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                     app = docker.build("${REPO_NAME}:${env.BUILD_NUMBER}")
                }
            }
           
        }
        stage('Scan Image') {
            steps {
                sh "trivy image ${REPO_NAME}:${env.BUILD_NUMBER}"
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry("${ECR_NAME}", 'ecr:us-east-1:jenkinsuser') {
                    app.push("${env.BUILD_NUMBER}")
                }
              }
            }
            
        }
        
        
    }

}
