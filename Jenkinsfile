node{

    def mavenHome = tool name: "maven 3.8.6"
    
    echo "Build Number: ${env.BUILD_NUMBER}"
    echo "Node Name: ${env.NODE_NAME}"
    echo "Job Name: ${env.JOB_NAME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])

    stage('CheckoutCode'){
        git credentialsId: 'd2a218c3-f497-4b22-aa9c-9b434ec2d969', url: 'https://github.com/devops-maven-projects/maven-web-application.git'
    }
    
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('ExecutingSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadArtifactsToNexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('DeployappintoTomcatServer'){
        sshagent(['b3f9454e-9fbe-43a5-8f2f-5d43d20f4823']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.14.107:/opt/apache-tomcat-9.0.69/webapps/"
    }
    }
}
