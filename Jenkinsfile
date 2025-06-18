pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'anup011/aws-end-to-end-cicd'  // Your DockerHub repo
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Anupj11/project-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE ./docker'
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                        docker stop webapp || true
                        docker rm webapp || true
                        docker run -d -p 8081:80 --name webapp $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'endtoend', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sh '''
                    cd Ansible
                    ansible-playbook -i Hosts playbook.yml
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
