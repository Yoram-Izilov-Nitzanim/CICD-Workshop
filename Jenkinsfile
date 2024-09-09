pipeline {
  agent any
  triggers {
    pollSCM('H/5 * * * *') // Polling interval
  }
  stages {
    stage('Checkout') {
      steps {
        script {
          // Checkout the code from the Git repository
          checkout scm
          
          // Check if there are any changes
          def changes = sh(script: "git diff --name-only HEAD^ HEAD", returnStdout: true).trim()
          if (changes) {
            env.HAS_CHANGES = 'true'
          } else {
            env.HAS_CHANGES = 'false'
          }
        }
      }
    }
    stage('Deploy') {
      when {
        expression {
          return env.HAS_CHANGES == 'true'
        }
      }
      steps {
        // ansible path - ansible-job   
        withCredentials([aws(credentialsId: 'my-aws-credentials-id', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            sh 'ansible-playbook /var/lib/jenkins/workspace/ansible-job/ansible/create-infra.yaml'
        }
      }
    }
  }
  post {
    success {
      echo 'Deployment succeeded!'
    }
    failure {
      echo 'Deployment failed!'
    }
  }
}
