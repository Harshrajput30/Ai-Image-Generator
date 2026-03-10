pipeline{
    agent any
     
    stages{

        stage("Code"){
            steps{
                git url:"https://github.com/Harshrajput30/Ai-Image-Generator", branch:"main"
            }
        }

        stage("Build"){
            steps{
                sh 'docker build -t ai-image-generator .'
            }
        }

        stage("DockerHub Login"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage("Push"){
            steps{
                sh 'docker tag ai-image-generator harshrajputt/ai-image-generator:latest'
                sh 'docker push harshrajputt/ai-image-generator:latest'
            }
        }

        stage("Deployment"){
            steps{
                sh 'docker rm -f ai-container || true'
                sh 'docker run -d -p 80:80 --name ai-container harshrajputt/ai-image-generator:latest'
            }
        }

    }
}
