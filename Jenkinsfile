node{
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '6', daysToKeepStr: '', numToKeepStr: '6')), pipelineTriggers([pollSCM('* * * * *')])])
 def mavenHome = tool name : 'maven-3.9.6'
 stage('CheckoutSourceCode'){
 git branch: 'dev', changelog: false, credentialsId: '30fa64e3-9387-493b-81c6-55022a195554', 
 poll: false, url: 'https://github.com/raviuptimecareer/maven-web-application.git'
 }
 stage('BuildArtifact'){
 sh "${mavenHome}/bin/mvn clean package"
 }
 stage('Report SonarQube'){
 sh "${mavenHome}/bin/mvn clean sonar:sonar"
 }
 stage('UploadArtifact Into Nexus'){
 sh "${mavenHome}/bin/mvn clean deploy"
 }
 stage('Deploy App Into Tomcat'){
sshagent(['42b59a53-2b45-4a7b-9c55-e1342fad6653']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.232.112:/opt/tomcat9/webapps"
}
}
}
