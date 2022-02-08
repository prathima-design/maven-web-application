node{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
    pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.8.4"
    
    //get the code from GitHub Repo
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: 'cd8afdae-48c8-400f-abe7-6263130ca60c', url: 'https://github.com/prathima-design/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactIntoNexusRepo'){
      sh "${mavenHome}/bin/mvn deploy"  
    }
    stage('DeployApplicationIntoTomcatServer'){
        sshagent(['44060080-69da-471b-ae29-c0a21560b0db']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.109.143.244:/opt/apache-tomcat-9.0.58/webapps"
}
    }
}
