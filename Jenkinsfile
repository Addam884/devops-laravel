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

    stage("Deploy"){
    docker.image('alpine').inside('-u root') {

        sh '''
        apk add --no-cache rsync openssh
        '''

        sshagent(credentials: ['ssh-prod']) {

            sh 'mkdir -p ~/.ssh'
            sh 'ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts'

            sh '''
            rsync -rav --delete ./ \
            ubuntu@$PROD_HOST:/home/adam/ansible/prod.kelasdevops.xyz/ \
            --exclude=.env \
            --exclude=storage \
            --exclude=.git
            '''
        }
    }
}

}