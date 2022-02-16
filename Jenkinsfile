pipeline {
  agent none
  stages {
    stage('Compilar') {
      agent {
        label 'maven'
      }
      steps {
        echo 'At this point  we should use maven or a tool to compile'
        sh 'mvn -B -DskipTests clean package'
      }
    }
  }
}
