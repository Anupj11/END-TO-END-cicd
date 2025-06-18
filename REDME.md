# DevOps Static Website Deployment

This project demonstrates a CI/CD pipeline using:

- **Jenkins**: Orchestrates the pipeline
- **Docker**: Builds and runs a static website container
- **Ansible**: Deploys the Docker container on a remote EC2 instance

## Structure

- `docker/`: Contains static HTML and Dockerfile
- `Ansible/`: Contains playbook and hosts inventory
- `Jenkinsfile`: CI/CD pipeline definition
