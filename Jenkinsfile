pipeline {
  agent none
  tools {
      maven 'maven'
  }
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
        script {
          def example1() {
            println "This is written from a groovy script inside a step to see if this works"
          }
          example1()
        }
      }
    }
    
    stage("Fin") {
      steps {
        echo "If we see this message printed is because everything worked, if not there is a mistake..."
      }
    }
  }
}
