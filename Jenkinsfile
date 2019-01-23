
pipeline {
  agent {
    kubernetes {
        label 'docker-build-pod'
        yamlFile 'podTemplate/spring-petclinic.yaml'
    }
  }
  stages {
    stage('Maven Install') {
      steps {
        container('maven') {
          sh 'mvn clean install'
        }
      }
    }
    stage('Docker Build') {
      steps {
        container('docker'){
          sh 'docker build -t stefku/spring-petclinic:latest .'
        }
      }
    }
    stage('Docker Push') {
      steps {
        container('docker'){
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
            sh 'docker push dcanadillas/spring-petclinic:latest'
          }
          publishEvent simpleEvent('skures/spring-petclinic:latest')
        }
      }
    }
  }
}
