pipeline {
  agent agent1

  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Build') {
      steps {
        script {
          if (isUnix()) {
            sh '''
              rm -rf out
              mkdir -p out
              javac -d out src/hello/Hello.java
            '''
          } else {
            bat '''
              if exist out rmdir /s /q out
              mkdir out
              javac -d out src\\hello\\Hello.java
            '''
          }
        }
      }
    }

    stage('Run') {
      steps {
        script {
          if (isUnix()) {
            sh '''
              java -cp out hello.Hello
              echo "Build_OK" > artifact.txt
            '''
          } else {
            bat '''
              java -cp out hello.Hello
              echo Build_OK > artifact.txt
            '''
          }
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'artifact.txt, out/**', allowEmptyArchive: false
    }
  }
}