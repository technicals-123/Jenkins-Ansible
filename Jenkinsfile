pipeline {
    agent any

    parameters {
        choice(name: 'services', choices: ['nginx', 'tomcat', 'both'], description: 'Select services to run')
    }

    environment {
        SAUMYA_SSH_KEY = credentials('SAUMYA_SSH_KEY')
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'SAUMYA_SSH_KEY', keyFileVariable: 'SSH_KEY')]) {
                        if (params.services == 'nginx' || params.services == 'both') {
                            sh '''
                                ansible-playbook -i my_ansible_project/inventory/hosts my_ansible_project/install_services_playbook.yml --tags=nginx --private-key=$SSH_KEY 
                            '''
                        }
                        if (params.services == 'tomcat' || params.services == 'both') {
                            sh '''
                                ansible-playbook -i my_ansible_project/inventory/hosts my_ansible_project/install_services_playbook.yml --tags=tomcat --private-key=$SSH_KEY 
                            '''
                        }
                    }
                }
            }
        }
    }
}
