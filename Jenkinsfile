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
        stages("Build Application"){
            stages{
                sh "mvn clean package"
            }
        }
        stages("Test Application"){
            steps{
                sh"mvn test"
            }
        }
        

    }
    
}