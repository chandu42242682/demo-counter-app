pipeline {

    agent any

    stages {
        
        stage('Git Checkout'){
           
           steps{
               git branch: 'main', url: 'https://github.com/chandu42242682/demo-counter-app'
            }
        }
        stage('Unit Testing'){
           
           steps{
               sh 'mvn test'
            }
        }
        stage('Integration Testing'){
           
           steps{
               sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build'){

           steps{
               sh 'mvn clean install'
            }
        }
        stage('static code analysis'){

           steps{
               script{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh 'mvn clean package sonar:sonar'
                  }
                }
            }
        }
        stage('Quality Gate Status'){

           steps{
             
               script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
               }
            }
        }
        stage('upload war file to nexus'){

            steps{

                script{
                    
                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readPomVersion.version.endswith("SNAPSHOT") ? "jenkins-snapshot" : "jenkins-release"
                    
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '54.236.42.81:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'nexusRepo', 
                    version: "${readPomVersion.version}"
                }
            }
        }
    }
}
 
            
    
        
    
