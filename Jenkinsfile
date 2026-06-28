pipeline {
  agent any
  stages {
    stage('Build') {
      stages {
        stage('Clean') {
          steps {
            script {
              if (fileExists('build')) {
                echo 'build exists; removing'
                sh 'rm -rf build'
                echo 'build removed'
              } else {
                echo 'no build exists'
              }
            }
          }
        }
        stage('Compile') {
          steps {
            sh 'cmake -B build -S .'
            sh 'cmake --build build'
          }
        }
      }
    }
  }
}
