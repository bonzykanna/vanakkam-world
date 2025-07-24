
 
pipeline {

    agent any
 
    tools {

        maven 'NarenMaven'    // Use the Maven tool configured in Jenkins > Global Tools

    }
 
    environment {

        IMAGE_NAME = "bonzykanna/java-app"

        IMAGE_TAG = "1.0"

    }
 
    stages {

        stage('Checkout') {

            steps {

                git url: 'https://github.com/bonzykanna/vanakkam-world.git' // Replace with actual repo

            }

        }
 
        stage('Build with Maven') {

            steps {

                sh 'mvn clean package'

            }

        }
 
        stage('Docker Build') {

            steps {

                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'

            }

        }
 
        stage('Docker Login & Push') {

            steps {

                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {

                    sh '''

                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin

                        docker push $IMAGE_NAME:$IMAGE_TAG

                    '''

                }

            }

        }
 
        stage('Run Container') {

            steps {

                sh '''

                    docker stop myapp || true

                    docker rm myapp || true

                    docker run -d --name myapp -p 9080:8080 $IMAGE_NAME:$IMAGE_TAG

                '''

            }

        }

    }
 
    post {

        always {

            echo "✅ Pipeline Finished! yahoooooooooooooo!!!!!!!!!!!!!!! Super!!!!!!!!!!1"

        }

    }

}
 
