pipeline {
  environment {
    registry = "faziriya/firstrepo"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages { 
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    /*stage('SonarQube analysis') {
        environment {
            scannerHome = tool 'SonarQube_4.3.0'
        }
        steps {
            withSonarQubeEnv('Your Sonar Server Name here') {
                sh '''
                ${scannerHome}/bin/sonar-scanner \
                -D sonar.projectKey=YOUR_PROJECT_KEY_HERE \
                -D sonar.projectName=YOUR_PROJECT_NAME_HERE \
                -D sonar.projectVersion=YOUR_PROJECT_VERSION_HERE \
                -D sonar.languages=js,ts \  // DEPRECATED, do not use this option
                -D sonar.sources=./src \
                -D sonar.test.inclusions=YOUR_INCLUSIONS_HERE \
                -D sonar.exclusions=YOUR_EXCLUSIONS_HERE
                '''
            }
        }
    }*/
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
