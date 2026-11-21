pipeline {
    agent any

    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fatmaghribi147852369-art/Ecommerce-DevOps.git'
            }
        }

        stage('Maven Build') {
            steps {
                dir('backend') {
                    sh 'docker run --rm -v "$PWD:/app" -v maven-cache:/root/.m2 -w /app maven:3.9-eclipse-temurin-22 mvn -B -ntp clean package -DskipTests'
                }
            }
        }

        stage('Building Image') {
            steps {
                dir('backend') {
                    sh 'docker build -t ecommerce-backend:1.0 .'
                }
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'de439775-53ff-4e9a-b677-952c2e736b00',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag ecommerce-backend:1.0 fatmaghribi123/ecommerce-backend:1.0
                        docker push fatmaghribi123/ecommerce-backend:1.0
                        docker logout
                    '''
                }
            }
        }
        stage('Docker Compose') {
            steps {
                sh 'docker compose --env-file /var/lib/jenkins/ecommerce.env up -d'
            }
        }
    }
}
