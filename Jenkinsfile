pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                    nexusArtifactUploader artifacts: 
                    [[artifactId: 'simple-app', classifier: '', file: 'tatget/simple-app-3.0.0.war', type: 'war']],
                     credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '13.52.241.133', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://ec2-13-52-241-133.us-west-1.compute.amazonaws.com:8081/repository/simpleapp1-release/', version: '3.0.0'
                    }
            }
        }
    }
}
