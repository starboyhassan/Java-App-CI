pipeline{
    agent{
        label "Jenkins-Agent"
    }

    environment {
        ECR_REPO = '767397888237.dkr.ecr.us-east-1.amazonaws.com/java-project-repo'
        APP_IMAGE_NAME = 'Java-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    tools{
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{
        stage("CleanUp Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from Github"){
            steps{
                git branch: 'main', url: 'https://github.com/StarboyHassan/Java-App-CI'
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test Application"){
            steps{
                sh"mvn test"
            }
        }
        // stage("SonarQube Analysis"){
            // steps{
//                 script{
 //                    WithSonarQubeEnv(credentialsID: 'jenkins-sonarqube-token'){
//                         sh"mvn sonar:sonar"
//                     }
//                 }
//             }
   //      }

//         stage("Quality Gate"){
//            steps{
//                script{
//                    waitForQualityGate abortPipeline: false; credentialsID: 'jenkins-sonarqube-token'
//                }
//            }
//        }

        stage('Build Docker Image') {
            steps {
                // build and tag images to push them to ECR
                sh "sudo docker build -t ${ECR_REPO}:${APP_IMAGE_NAME}-${IMAGE_TAG} ."
            }

        }
        stage('Push Image to ECR') {
            steps {
                withAWS(credentials: "${AmazonWebServicesCredentials-ECR-EKS}"){
                    sh "(aws ecr get-login-password --region us-east-1) | docker login -u AWS --password-stdin ${ECR_REPO}"
                    sh "docker push ${ECR_REPO}:${APP_IMAGE_NAME}-${IMAGE_TAG}"
                }
            }

        }        

        
    }
    
}