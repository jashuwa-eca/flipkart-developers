node("Node1")
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.6.3"
    stage("checkoutthecode")
    {
        git branch: 'development', credentialsId: '7e779fc8-aeae-4ac6-a588-6500557694e6', url: 'https://github.com/jashuwa-eca/maven-web-application.git'
    }
    stage("Build")
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage("ExecutetheSonarQubeReport")
    {
        sh "${mavenHome}/bin/mvn clean package sonar:sonar"
    }
    stage("UploadtheArtifactintoNexusRepository")
    {
        sh "${mavenHome}/bin/mvn clean package deploy"
    }
    stage("DeployintoTomcat")
    {
        sshagent(['c8bea48d-fd87-4870-a06c-32ae7aa28554']) 
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.204.30.247:/opt/apache-tomcat-9.0.62/webapps/"
}
    }
    stage("SendEmailNotification")
    {
        emailext body: '''Build is over

Regards,
Jashuwa VR.''', subject: 'Build is over', to: 'krisjash999@gmail.com'
    }
}
