node{   
    
def mavenHome = tool name: "maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Node name is: ${env.NODE_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"


try{
stage('CheckoutCode')
{
git branch: 'development', credentialsId: '56852f69-6a47-421e-8bbe-7275ec1b6a7f', url: 'https://github.com/venkateshbs723/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
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
