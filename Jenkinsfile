pipeline {
  agent any
  stages {
    stage('Backup') {
      steps {
        sh """
            JOB_DIR=\$(pwd)
            rm -rf backup
            mkdir backup
            cd /var/lib/jenkins/jobs
            find . -type f -name config.xml -exec cp --parent {} \$JOB_DIR/backup \\;
            cd \$JOB_DIR
            git add .
            git commit -m 'Backup of Jenkins jobs'
            """
      }
    }
    stage('Push to repo') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'df1cdf5a-49e4-45f1-848d-01d5cf179798', keyFileVariable: 'SSH_KEY')]) {
          withEnv(["GIT_SSH_COMMAND=ssh -o StrictHostKeyChecking=no -i ${SSH_KEY}"]) {
              sh 'git push origin HEAD:master'
          }
        }
      }
    }
  }
}
