pipeline {
  agent none
  options { timestamps(); timeout(time: 15, unit: 'MINUTES') }

  stages {
    stage('Where am I (Controller)') {
      agent { label 'built-in' }
      steps {
        echo "NODE = ${env.NODE_NAME}"
        echo "WORKSPACE = ${env.WORKSPACE}"
      }
    }

    stage('Build on Unix Agent') {
      agent { label 'built-in' } // or another Unix label
      steps {
        sh '''
          rm -rf out
          mkdir -p out
          javac -d out src/hello/Hello.java
          java -cp out hello.Hello
          echo Build_OK > artifact.txt
        '''
      }
      post {
        always {
          archiveArtifacts artifacts: 'artifact.txt, out/**', allowEmptyArchive: false
        }
      }
    }

    stage('Parallel: Controller vs Agent') {
      parallel {
        stage('Controller lane') {
          agent { label 'built-in' }
          steps {
            echo "PARALLEL NODE = ${env.NODE_NAME}"
          }
        }
        stage('Agent lane') {
          agent { label 'built-in' } // another Unix node/label if you have one
          steps {
            sh 'echo "PARALLEL NODE = $NODE_NAME"'
          }
        }
      }
    }
  }
}