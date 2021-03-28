pipeline{
    agent {
        label 'master'
    }
    stages{
        stage('Checkout SCM'){
            steps{
                git 'https://github.com/rahuldevops1/devopsproject1-1.git'
            }
        }
        stage('Build FrontEnd'){
            steps{
                //sh './mvnw'
                sh 'npm install'
                sh 'npm build'
            }

        }
        stage('Build BackEnd'){
            steps{
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
            
        }
         stage('Build Image') {
            steps {
                sh "docker image build . -t csdeol100/proj1"
            }
        }
        stage('Publish Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                  sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                  sh "docker image push csdeol100/proj1"
                }
            }
        }
        stage('Deploy on AWS'){
            steps{
                sshagent(['awsagent']) {
                    sh 'ssh -o StrictHostKeyChecking=no -l ubuntu 54.86.131.60 uname -a'
                    sh 'ssh -v ubuntu@54.86.131.60 docker container run -idt -p 8080:8080 csdeol100/proj1'
                  

                }
            }
        }

    }
}
