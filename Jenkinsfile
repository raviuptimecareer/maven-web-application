node{
/*    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([cron('* * * * *')])])*/    def mavenHome = tool name: 'maven-3.9.6'
 stage('CheckoutCode'){
  git branch: 'development', changelog: false, credentialsId: '58ee9fff-c7eb-4a8a-afdf-5ef16ed7b243', poll: false, 
  url: 'https://github.com/raviuptimecareer/maven-web-application.git'
 }
 stage('BuildArtifact'){
 sh "${mavenHome}/bin/mvn clean package"
 }
 stage('SonarQubeReport'){
 sh "${mavenHome}/bin/mvn clean sonar:sonar"
 }
 Stage('UploadArtifact Into Nexus'){
 sh "${mavenHome}/bin/mvn clean deploy"
 }
 stage('Deploy App Into Tomcat'){
 sshagent(['ce6b46e3-ba8a-45cb-8146-1644067cd75e']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.232.112:/opt/tomcat9/webapps"
}
}
 stage('Send Email Notifications')
{
mail bcc: '', body: '''welcome to scripted pipeline in jenkins
we are prceed with email notification.
regards
ravikala raveendra''', cc: '', from: '', replyTo: '', subject: 'build done here', to: 'ravindra.ravikala.1236@gmail.com'
}
}
