pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        // use SSH repo (Jenkins already used SSH earlier)
        git url: 'git@github.com:Evanjalicloud1/jenkins-task.git', branch: 'main'
      }
    }

    stage('Build') {
      steps {
        sh '''
          echo "Building project..."
          echo "List workspace:"
          ls -l | tee build-output.txt
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          echo "Running tests..." | tee -a build-output.txt
          # add real test commands here, append output to build-output.txt
        '''
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          echo "Deploying app to server..." | tee -a build-output.txt
          nohup python3 -m http.server 8081 >/dev/null 2>&1 &
          echo "Deployed (http.server on 8081)" | tee -a build-output.txt
        '''
      }
    }
  }

  post {
    always {
      // archive the build output
      archiveArtifacts artifacts: 'build-output.txt', fingerprint: true

      // send email (from set to match SMTP user)
      emailext(
        subject: "Jenkins: ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
        to: "evanjalievanjali@gmail.com",
        from: "evanjalievanjali@gmail.com",
        body: """Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
Result: ${currentBuild.currentResult}
Build URL: ${env.BUILD_URL}

Console (last 200 lines):
${'$'}{BUILD_LOG, maxLines=200, escapeHtml=false}
""",
        attachmentsPattern: 'build-output.txt',
        mimeType: 'text/plain'
      )
    }
  }
}
