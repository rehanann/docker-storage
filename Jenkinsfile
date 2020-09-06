pipeline{
    agent any
    parameters {
        choice(name: 'name', choices: ['dev', 'prod', 'masters'], description: 'Env stack parameter')
    }
    stages{
            stage('Checkout') {
                steps{
                    checkout scm
                }
            }
            stage('deploy Overlayfs Graph driver on Dev Compute nodes') {
                when {
                    expression { params.name == 'dev' }
                    }
                    steps {
                        sh 'ansible-playbook -i host.ini docker-storage-setup-ofs.yml -e "name=dev"'
                    }
            }
            stage('deploy Overlayfs Graph driver on Prod Compute nodes') {
                    when {
                        expression { params.name == 'prod' }
                    }
                    steps {
                        sh 'ansible-playbook -i host.ini docker-storage-setup-ofs.yml -e "name=prod"'
                }
            }
            stage('deploy Overlayfs Graph driver on Master') {
                    when {
                        expression { params.name == 'masters' }
                    }
                        steps {
                            sh 'ansible-playbook -i host.ini docker-storage-master-setup-ofs.yaml -e "name=masters"'
                        }
                }
            stage('Post test') {
                steps {
                    sh 'kubectl get nodes'
                }
            }
        }
    }