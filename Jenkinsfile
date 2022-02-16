pipeline {
  agent none
  stages {
    stage('Compilar') {
      agent {
        label 'maven'
      }
      steps {
        echo 'At this point  we should use maven or a tool to compile'
        sh 'ls -lorth'
        echo "Above is a list of files in current directory"
        sh 'mvn -B -DskipTests clean package'
      }
    }
  }
}
