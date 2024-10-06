pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                git([url: 'https://github.com/RitikPyCode/django-notes-app.git', branch: 'main',])
            }
        }
        stage('Build'){
            steps{
                sh 'docker build -t my-note-app .'
            }
        }
        stage('Push to DockerHub'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker-credentials", passwordVariable: "dockerhubPass", usernameVariable:"dockerhubUser")]){
                    sh "docker tag my-note-app ${env.dockerhubUser}/my-note-app:latest"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker push ${env.dockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
