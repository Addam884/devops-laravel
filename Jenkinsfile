node {
    checkout scm

    stage("Build"){
        docker.image('composer:2').inside('-u root') {
            sh 'composer install'
        }
    }

    stage("Test"){
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage('Deploy') {
        sshagent(['adam']) {
            sh '''
            apt update
            apt install -y ansible
            ansible-playbook -i /home/adam/ansible/dev.kelasdevops.xyz/hosts deploy.yml
            '''
        }
    }
}
