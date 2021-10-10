pipeline {
    environment {
        registry = "vijayrajan23/mywebapp"
        registryCredential = "4f674f88-6d40-4315-b944-99a50a81e7dd"
        dockerImage = ''
        containerName = "mywebapp"
    }
    agent {label 'slave'}
    stages {
        stage('Clone for GIT') {
            steps {
                git branch: 'main', url: 'https://github.com/vijayrajan23/dock-test.git' 
            }
        }
        stage('build docker image'){
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }

            }
        }
        stage('upload Image') {
            steps {
                script {
                    docker.withRegistry( '' , registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }
        stage('stop to runing container') {
            steps {
                sh 'docker ps -f name=$containerName -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -f name=$containerName -q | xargs -r docker container rm'
            }
        }
        stage('start container') {
            steps {
               sh "docker run -d -p 8080:80 --name $containerName  $registry:$BUILD_NUMBER"
            }
        }
        
    }
}
