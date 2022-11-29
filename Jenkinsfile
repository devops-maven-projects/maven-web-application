pipeline{  // pipeline start
    
    agent any

    tools{  // tools start
        maven "maven 3.8.6"
    } //tools end

    stages{  // stages start

        stage('CheckoutCode'){  // checkout satge start
            steps{
                sendslacknotifications("STARTED")
                git credentialsId: 'd2a218c3-f497-4b22-aa9c-9b434ec2d969', url: 'https://github.com/devops-maven-projects/maven-web-application.git'
            }
        }  // checkout stage end
        stage('Build'){  // build stage start
            steps{
                sh "mvn clean package"
            }  
        }  // build stage end
        stage('ExecutingSonarQubeReport'){  // sonar stage start
            steps{
                sh "mvn sonar:sonar"
            }
        }  // sonar stage end
        stage('UploadArtifactsToNexus'){  // nexus stage start
            steps{
                sh "mvn deploy"
            }
        }  // nexus stage end
        stage('DeployappintoTomcatServer'){  // deploy app stage start
            steps{
                sshagent(['b3f9454e-9fbe-43a5-8f2f-5d43d20f4823']) {
                       sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.14.107:/opt/apache-tomcat-9.0.69/webapps/"
                }
            }
        } // deploy stage end
    } // stages end
    // post stage starts
    post {
      unstable {
        sendslacknotifications(currentBuild.result)
      }
      success {
        sendslacknotifications(currentBuild.result)
      }
      failure {
        sendslacknotifications(currentBuild.result)
      }
    } // post stage ends
} // pipeline ends

// defining function
def sendslacknotifications(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#FFFF00'
    } else if (buildStatus == 'SUCCESS') {
        color = '#00FF00'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FF0000'
    } else {
        color = '#FF0000'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

    slackSend(color: color, message: msg, channel: "#jenkinsbuildnotifications")
}  // ending of function
