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
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                    sh 'mvn clean package sonar:sonar'
                  }
                }
            }
        }
        stage('Quality Gate Status'){

           steps{
             
               script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
               }
            }
        }
        stage('upload war file to nexus'){

            steps{

                script{
                    
                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "jenkins-snapshot" : "jenkins-release"
                    
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
                    nexusUrl: '18.214.15.254:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'nexusRepo', 
                    version: "${readPomVersion.version}"
                }
            }
        }
        stage('Docker Image Build'){

            steps{

                script{

                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID anushadasari/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID anushadasari/$JOB_NAME:latest'
                }
            }
        }                   
    }        
}        
        
 
            
    
        
    
