pipeline {
    
    agent any
    
    stages {
        stage("code"){
            steps {
                echo "cloning the code"
                git url:"https://github.com/dheepak240488/django-notes-app.git" , branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Build the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag my-note-app ${env.dockerhubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/my-note-app:latest"
                }

            }
        }
        stage("deploy"){
            steps {
                echo "deploy te container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
