pipeline {
    agent none
    environment {
        DOCKERHUB_CREDENTIALS=credentials('66e2dad9-5f41-41ab-a9d8-e7b7ae5b501f')
    }
    stages {
        stage('git') {
            agent {
                label 'k8node'
            }
            steps {
                git 'https://github.com/Rathan-Gemanjyothi/nginx.git'
            }
        }
        stage('docker') {
            agent {
                label 'k8node'
            }
            steps {
                sh 'sudo docker build /home/ubuntu/jenkins/workspace/newjob -t rathangemnajyothi/project'
                sh 'sudo echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'sudo docker push rathangemanjyothi/project'
            }
        }
        stage('kubernetes') {
            agent {
                label 'k8node'
            }
            steps {
               sh 'kubectl delete deploy nginx-deployment'
               sh 'kubectl apply -f deployment.yaml'
               sh 'kubectl delete service my-service'
               sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
