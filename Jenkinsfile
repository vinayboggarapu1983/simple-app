pipeline {
    agent any
    tools {
        maven 'Maven3.8.6'
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
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus_cred', 
                    groupId: 'in.javahome', 
                    nexusUrl: '34.88.165.112:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexus_repo, 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
