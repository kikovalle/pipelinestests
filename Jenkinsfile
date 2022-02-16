pipeline {
  agent any
  stages {
    stage('Compilar') {
      agent {
        docker {
          image 'maven:latest'
          label 'mavendocker'
        }
      }
      steps {
        echo 'At this point  we should use maven or a tool to compile'
        sh 'mvn -B -DskipTests clean package'
      }
    }
  }
}
