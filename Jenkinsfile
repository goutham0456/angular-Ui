pipeline {
     agent any
     environment {
        dockerImage = ''
        registry = 'gawtham/angularuiapp'
        registryCredential ='dockerhub_id'
    }
    stages {
        stage ('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_id', url: 'https://github.com/goutham0456/angular-Ui.git']]])
            }
        }
         
        stage('Build Docker Images') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Upload to DockerHub'){
            steps {
                script {
                        docker.withRegistry('',registryCredential){
                            dockerImage.push()
                        }
                }
            }
        }
        stage('docker stop container') {
         steps {
            sh 'docker ps -f name=practical_newton1  -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=practical_newton1 -q | xargs -r docker container rm'
         }
       }
    //   stage('Approval') {
    //         input {
    //         message "Proceed to deploy?"
    //          ok "YES"
    //       }
    //   }   
       stage('Docker Run') {
        steps {
            script {
                dockerImage.run("-p 8020:80 --rm --name practical_newton1")
                }
            }
        }
    }
}
