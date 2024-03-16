pipeline{
    agent{
        label "Jenkins-Agent"
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
        stage("SonarQube Analysis"){
            steps{
                script{
                    WithSonarQubeEnv(credentialsID: 'jenkins-sonarqube-token'){
                        sh"mvn sonar:sonar"
                    }
                }
            }
        }

        stage("Quality Gate"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false; credentialsID: 'jenkins-sonarqube-token'
                }
            }
        }


    }
    
}