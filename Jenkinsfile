pipeline {
  agent any

  environment {
    BUILD_DIR = 'build'
  }

  stages {

    stage('Pre') {
      stages {

        stage('Clean') {
          steps {
            script {
              if (fileExists('${BUILD_DIR}')) {
                echo 'build exists; removing'
                sh "rm -rf ${BUILD_DIR}"
                echo 'build removed'
              } else {
                echo 'no build exists'
              }
            }
          }
        }

        stage('Configure Build Dependencies') {
          steps {
            sh 'conan profile detect --force'
          }
        }
      
      }
    }

    stage('Build') {
      steps {
        sh "conan install . --output-folder=${BUILD_DIR} --build=missing"
        sh "cmake . -DCMAKE_TOOLCHAIN_FILE=${BUILD_DIR}/conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release -B ${BUILD_DIR}"
        sh "cmake --build ${BUILD_DIR}"
      }
    }
    
    stage('Test') {
      parallel {

        stage('Unit Test') {
          steps {
            sh "./${BUILD_DIR}/casino_game"
            sh "./${BUILD_DIR}/test_game"
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
        sh "tar -czf casino_game.tar.gz ${BUILD_DIR}/casino_game"
        archiveArtifacts artifacts: 'casino_game.tar.gz', fingerprint: true
      }
    }

  }
}
