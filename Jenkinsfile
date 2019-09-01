pipeline {
  agent { label 'slave' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  tools {
    nodejs 'node'
  }
  stages {
    stage('npm install'){
      steps{
         sh "npm install"
      }
    }
    stage('npm build'){
      steps{
        sh "npm run build"
      }
    }
    stage('docker build'){
      steps{
        script {
          docker.build('portabledave/simple-web-server')
        }
      }
    }
    stage('docker push'){
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', '822d4685-d1fe-4628-a373-84744cdb8327') {
            docker.image('portabledave/simple-web-server').push('latest')
          }
        }
      }
    }
  }
  post {
    always {
      sh 'echo "This will always run"'
    }
  }
}
