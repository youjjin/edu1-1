properties([pipelineTriggers([githubPush()])])

pipeline {
    environment {
        // Global 변수 선언
        dockerRepo = "211.43.12.120:40010/edu1"
        dockerCredentials = 'nexus_ci_edu3'
        dockerImageVersioned = ""
        dockerImageLatest = ""
    }

    agent any

    stages {
        /* checkout repo */
        stage('Checkout SCM') {
            steps{
                script{
                    checkout scm
                 }
            }   
        }
        
        stage("Building docker image"){
            steps{
                script{
                    dockerImageVersioned = docker.build dockerRepo //+ ":$BUILD_NUMBER"
                    dockerImageLatest = docker.build dockerRepo + ":latest"
                }
            }
        }
        stage("Pushing image to registry"){
            steps{
                script{
                    // if you want to use custom registry, use the first argument, which is blank in this case
                    docker.withRegistry( 'http://211.252.85.148:40010', dockerCredentials){
                        dockerImageVersioned.push()
                        dockerImageLatest.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $dockerRepo"//:$BUILD_NUMBER"
            }
        }
    }

    /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
   }
}
