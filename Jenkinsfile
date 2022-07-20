node{   
    
def mavenHome = tool name: "maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Node name is: ${env.NODE_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"


try{
sendSlackNotifications('STARTED')
    
stage('CheckoutCode')
{
git branch: 'development', credentialsId: '56852f69-6a47-421e-8bbe-7275ec1b6a7f', url: 'https://github.com/venkateshbs723/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
    
stage('TriggerDownstreamJob'){
 build Job: 'Pipeline pipelinescriptwithbuildparameters'   
}
        
/*
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer')
{
sshagent(['d8884ca1-a9db-414b-ac4a-ea35a965271f']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.32.172:/opt/apache-tomcat-9.0.63/webapps" 
}

}

*/
}catch(e){
currentBuild.result = "FAILED"
    throw e
}
finally{
sendSlackNotifications(currentBuild.result)
}
    
}//Node Closing

//Belode code will use for send slack build notifications

def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel:'walmart')
}


