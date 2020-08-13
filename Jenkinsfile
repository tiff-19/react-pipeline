pipeline {
    // agent {
    //     docker {
    //         image 'node:6-alpine'
    //         args '-p 3000:3000'
    //         args '-v $HOME/.m2:/root/.m2'
    //     }
    // }
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                echo 'start build'
                // nodejs('node'){
                    sh 'npm install'
                // }
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Build react project') {
            steps{
                sh 'npm run build'
            }
        }
        stage('Build docker image') {
            steps{
                script {
                	app = docker.build("tiff19/reactapp-test")
                }
            }
        }
        stage('Test docker image') {
            steps {
                sh 'docker run -d --rm --name testImages -p 8081:80 tiff19/reactapp-test'
                input message: "Finished test image? (Click proceed to continue)"
            }
        }
        stage('Clean up docker test') {
            steps {
                sh 'docker stop testImages'
            }
        }
    }
}
