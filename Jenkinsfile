pipeline {
    agent any
    /*environment{
        DOCKER_HUB_USERNAME = 'suve11'
        DOCKER_HUB_PASSWORD = 'Dhaaru@mmu11'
        APP_NAME = 'todo-application-image'
        APP_PORT = '8082'*/
    stages {

        stage('clone repository'){
            steps{
                git branch: 'master',
                  url: 'https://github.com/suveammu/todo-application.git'
            }
        }
        // stage('Checkout'){
        //     steps{
        //         checkout scm
        //     }
        // }
        stage('Build maven') {
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build docker') {
            steps{
                sh 'docker build -t suve11/todo-application-image:latest .'
            }
        }
        stage('Docker Push') {
            steps {
                withCRedentials([usernamePassword(
                    credentialsId: 'docker-hub-credentials',
                    usernameVariable: 'suve11',
                    passwordVariable: 'Dhaaru@mmu11'
                )])
                 {   
                    sh '''
                       docker login -u $suve11 --password-stdin
                      docker push $suve11/$todo_application_image:latest
                    '''
                }
            }
        }

        stage('Deploy'){
            steps{
                
                sh 'docker-compose up -d'
                sh 'docker ps'
            }
        }

        stage('cleanup'){
            steps {
                sh 'rm -rf *'
            }
        }
    }

    post{
        always{
            echo 'This runs regardless of success or failure'
        }
        success{
            echo 'Pipeline success'
        }
        failure{
            echo 'Pipeline failed'
        }
    }
}
