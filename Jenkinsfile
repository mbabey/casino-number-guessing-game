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
    
    stage('Test') {
      parallel {

        stage('Unit Test') {
          steps {
            sh './build/casino_game'
            sh './build/test_game'
          }
        }

        stage('SAST') {
          steps {
            echo 'mock SAST'
          }
        }

      }
    }

    stage('Package') {
      steps {
        sh 'tar -czf casino_game.tar.gz build/casino_game'
        archiveArtifacts artifacts: 'casino_game.tar.gz', fingerprint: true
      }
    }

  }
}
