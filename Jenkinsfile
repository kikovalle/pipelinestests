pipeline {
  agent any
  stages {
    stage('Compilar') {
      agent {
        kubernetes {
          cloud 'kubernetes'
          label 'kubernetes'
        }
      }
      steps {
        echo 'At this point  we should use maven or a tool to compile'
        sh 'mvn -B -DskipTests clean package'
      }
    }
  }
}
