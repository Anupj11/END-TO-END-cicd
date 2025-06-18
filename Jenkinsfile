pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Anupj11/project-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-static-site ./docker'
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                        docker stop webapp || true
                        docker rm webapp || true
                        docker run -d -p 8081:80 --name webapp devops-static-site
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
