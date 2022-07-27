node
{
    //Global Properties
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '1', daysToKeepStr: '', numToKeepStr: '10')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([githubPush(), pollSCM('* * * * *')])])
    buildName '${JOB_NAME}--BuildNo:${BUILD_NUMBER}'
    //Global Variables
    def MavenHome = tool name: "Maven-v3.8.6"
    stage('PullSrcCode')
    {
        git branch: 'development', credentialsId: '329f9f41-6a14-43a9-8926-8e505d021b2f', url: 'https://github.com/kvkalyan37/sample-proj.git'
    }
    stage('CreatePackage')
    {
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage('GenereateSonarQubeReport')
    {
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadBuildArtiact')
    {
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage('DeployBAontoAppServer')
    {
        deploy adapters: [tomcat9(credentialsId: 'f9e71713-9595-4e60-ad62-d1b21a0c5d7d', path: '', url: 'http://localhost:8880')], contextPath: '/Sample_Proj', war: '**/maven-web-application.war'
    }
    /*stage('MailNotification')
    {
        mail bcc: '', body: 'Hi Team....Your build is completed', cc: '', from: 'jenkins@gmail.com', replyTo: 'kurmanavenkatkalyan37@gmail.com', subject: 'Build Completed', to: 'kurmanavenkatkalyan37@gmail.com'
    }*/
}
