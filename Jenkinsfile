pipeline{
    agent{
        label "Jenkins-Agent"
    }

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '767397888237'
        ECR_REPO_NAME = 'java-project-repo'
        APP_IMAGE_NAME= 'java-app'
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
                script {
                    docker.build("your_image_name:${env.IMAGE_TAG}")
                }
            }
        }
        stage('Push to ECR') {
                    steps {
                        script {
                            withCredentials([[
                                $class: 'AmazonWebServicesCredentialsBinding',
                                credentialsId: 'AmazonWebServicesCredentials-ECR-EKS',
                                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                            ]]) {
                                docker.withRegistry("https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", 'ecr:AmazonWebServicesCredentials-ECR-EKS') {
                                    docker.image("${APP_IMAGE_NAME}:${env.IMAGE_TAG}").push("${ECR_REPO_NAME}:${env.IMAGE_TAG}")
                                }
                            }
                        }
                    }
                }       

        
    }
    
}