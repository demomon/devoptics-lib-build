pipeline {
  agent {
    kubernetes {
    label 'mypod'
    yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3-jdk-11-slim
    command: ['cat']
    tty: true
"""
        }
    }
  stages {
    stage('Checkout') {
      steps{
        git 'https://github.com/joostvdg/jx-maven-lib.git'
      }
    }
    stage('Build') {
      steps {
        // https://docs.cloudbees.com/docs/cloudbees-devoptics/latest/user-guide/value-streams#withmaven-pipeline-step
        // To use this feature, Jenkins must have both the DevOptics plugin and the Pipeline Maven plugin installed.
        container('maven') {
          script {
            withMaven() {
              sh "mvn clean install"
            }
          }
        }
      }
    }
    stage('Trigger App Build'){
      steps {
        build job: 'devoptics/devoptics-build/master', propagate: false, wait: false
      }
    }
  }
}
