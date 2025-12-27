pipeline {
  agent any

  stages {

    stage('Prepare SSH') {
      steps {
        withCredentials([
          sshUserPrivateKey(
            credentialsId: 'vm-ssh-key',
            keyFileVariable: 'SSH_KEY',
            usernameVariable: 'SSH_USER'
          )
        ]) {
          sh '''
            mkdir -p ~/.ssh
            cp "$SSH_KEY" ~/.ssh/id_rsa
            chmod 600 ~/.ssh/id_rsa

            ssh-keyscan -H 192.168.2.4 >> ~/.ssh/known_hosts
          '''
        }
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        sh '''
          cd ansible
          ansible-playbook -i inventory.ini diskUsageCheck.yml
        '''
        archiveArtifacts artifacts: 'ansible/df_output_*.txt', fingerprint: true
      }
    }
  }
}
