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
    }
}    
