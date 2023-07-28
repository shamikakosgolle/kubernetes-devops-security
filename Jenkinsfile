pipeline {
  agent master
 
  stages {

    stage('Build Artifact - Mavenc') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }




  }
}
