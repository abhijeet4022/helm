// Run the Helm Deployment for EK
pipeline {
  agent any
    // Ansi Colour options
  options {
    ansiColor('xterm')
    buildDiscarder(logRotator(numToKeepStr: '3'))
  }

  parameters {
    string(name: 'ENV', defaultValue: 'prod', description: 'Environment')
    string(name: 'APPNAME', defaultValue: '', description: '')
    string(name: 'APP_VERSION', defaultValue: '', description: '')
  }


  stages {
    stage('Get Kube Config') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-roboshop'
      }
    }

    stage('Get App Code for Variable'){
        steps{
            dir('APP'){
                git branch: 'main', url: 'https://github.com/abhijeet4022/${APPNAME}'
            }
            dir('CHART'){
             git branch: 'main', url: 'https://github.com/abhijeet4022/helm'
            }
        }
    }

    stage('Helm Deploy'){
        steps{
        sh 'helm upgrade -i ${APPNAME} ./CHART -f APP/helm/${ENV}.yaml --set APP_VERSION=${APP_VERSION}'
        }
    }
  }

}