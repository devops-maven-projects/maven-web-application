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
            sh "clean package"
        }
    }


}
