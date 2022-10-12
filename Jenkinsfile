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
    }
} 
        
    
