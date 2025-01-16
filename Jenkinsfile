pipeline {
  agent {
   label 'linux'
  }
  stages {
    stage('Checkout Code') {
        steps {
            sshagent(['a795e667-54e9-4f29-818e-f81c18219205']) {
                checkout scm
            }
        }
    }
    stage('Build') {
      steps {
        sh 'chmod a+x run_build_script.sh'
        sh './run_build_script.sh'
      }
    }
    stage('Test') {
      parallel {
        stage('Checkout Code') {
            steps {
                sshagent(['a795e667-54e9-4f29-818e-f81c18219205']) {
                    checkout scm
                }
            }
        }
       stage('Test On Windows') {
         agent {
           label 'window'
         }
         steps {
           echo "Running tests on Windows"
         }
       }
       stage('Test On Linux') {
         agent {
           label 'linux'
         }
         steps {
           echo "Running tests on Linux"
         }
       }
     }
    }
    stage('Confirm Deploy to staging') {
     steps {
       timeout(time: 60, unit: 'SECONDS') {
         input(message: 'Okay to Deploy?', ok: 'Let\'s Do it!')
       }
     }
    }
    stage('Deploy to Staging') {
     steps {
       echo "Deploying to staging..."
     }
    }
    stage('Confirm Deploy to production') {
     steps {
       timeout(time: 60, unit: 'SECONDS') {
         input(message: 'Okay to Deploy?', ok: 'Let\'s Do it!')
       }
     }
    }
    stage('Deploy to Production') {
     steps {
       echo "Deploying to production..."
     }
    }
  }
  post {
   success {
     echo "build succeeded"
   }
   failure {
     echo "Build failed"
   }
  }
}
