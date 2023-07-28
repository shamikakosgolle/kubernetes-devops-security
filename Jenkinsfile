pipeline {
  agent any
  stages {
    stage('Build Artifact - Mavencc') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }
  }
}
