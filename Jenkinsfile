pipeline{
    agent{
        label "Jenkins-Agent"
    }

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '767397888237'
        //ECR_REPO_NAME = 'java-project-repo'
        APP_IMAGE_NAME = 'java-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        ECR_REPO = '767397888237.dkr.ecr.us-east-1.amazonaws.com/java-project-repo'
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


        stage('Login to AWS') {
            steps {
                script {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'AmazonWebServicesCredentials-ECR-EKS',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                            sh "(aws ecr get-login-password --region us-east-1) | docker login -u AWS --password-stdin ${ECR_REPO}"
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                sh "docker build -t ${ECR_REPO}:${APP_IMAGE_NAME}-${IMAGE_TAG}  ."

                }
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${ECR_REPO}:${APP_IMAGE_NAME}-${IMAGE_TAG}"
                   }
            }
        }     

        
    }
    
}