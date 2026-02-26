pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker network create labnet || true'
                sh 'docker rm -f backend1 backend2 nginx-lb || true'
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh 'docker run -d --name backend1 --network labnet backend-app'
                sh 'docker run -d --name backend2 --network labnet backend-app'
                sh 'sleep 5'
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh 'docker run -d --name nginx-lb --network labnet -p 80:80 nginx'
                sh 'sleep 3'
                sh 'docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf'
                sh 'docker exec nginx-lb nginx -s reload'
            }
        }
    }
}
