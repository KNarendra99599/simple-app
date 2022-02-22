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
                    nexusArtifactUploader artifacts: [[artifactId: 'simple-app', classifier: '', file: 'target/simple-app-3.0.0.war', type: 'war']], credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '52.53.153.223', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://ec2-52-53-153-223.us-west-1.compute.amazonaws.com:8081/repository/maven-releases/', version: '3.0.0'
                    
                    }
            }
        }
    }
}
