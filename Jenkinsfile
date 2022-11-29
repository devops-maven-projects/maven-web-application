pipeline{
    
    agent any

    tools{
        maven "maven 3.8.6"
    }

    stages{

        stage('CheckoutCode'){
            steps{
                git credentialsId: 'd2a218c3-f497-4b22-aa9c-9b434ec2d969', url: 'https://github.com/devops-maven-projects/maven-web-application.git'
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean package"
            }
            
        }
        stage('ExecutingSonarQubeReport'){
            steps{
                sh "mvn sonar:sonar"
            }
        }
        stage('UploadArtifactsToNexus'){
            steps{
                sh "mvn deploy"
            }
        }
        stage('DeployappintoTomcatServer'){
            steps{
                sshagent(['b3f9454e-9fbe-43a5-8f2f-5d43d20f4823']) {
                       sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.14.107:/opt/apache-tomcat-9.0.69/webapps/"
                }
            }
        }
    }
}
