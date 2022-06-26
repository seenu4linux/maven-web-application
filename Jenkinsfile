node{
   def MavenHome = tool name:"maven3.8.5"
   echo "Branch name is: ${env.BRANCH_NAME}"
  // echo "Workaspace name is" ${env.WORKSPACE}"
   echo "JOB name is: ${env.JOB_NAME}"
   echo "Build id is: ${env.BUILD_ID}"
   echo "Node name is: ${env.NODE_NAME}"
   
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
   
   
    stage('CheckOutCode'){
        git branch: 'development', credentialsId: 'a853e7f2-1117-44b2-a703-21d3f8dfe7d4', url: 'https://github.com/seenu4linux/maven-web-application.git'
    }
    
    stage('Build'){
        sh "${MavenHome}/bin/mvn clean package"
    }
    
    stage('ExecuteSonarQubeReport'){
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadArtifactsInToNexusServer'){
     sh "${MavenHome}/bin/mvn deploy"
    }
    stage('DeployAppInToTomcatServer'){
        sshagent(['68531d99-b799-4f8d-87ed-16048ff9ea91']){
    // some block
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.9.249:/opt/apache-tomcat-9.0.63/webapps"
        }
    }
}
