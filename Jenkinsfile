pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker')
  }
  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jahcoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
        
          sh 'printenv'
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          sh 'docker build -t shamikakosgolle/madu:""$GIT_COMMIT"" .'
          sh 'docker push shamikakosgolle/madu:""$GIT_COMMIT""'
        
      }
    }
  }
}
