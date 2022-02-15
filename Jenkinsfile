pipeline {
  tools {
     dockerTool
  } 
  agent {
    label 'principal'
  }
  stages {
    stage('Code download') {
      steps {
        echo 'Start cloning repository to execute pipeline'
        git(url: 'https://github.com/kikovalle/pipelinestests', branch: 'main', changelog: true)
      }
    }

    stage('Compilar') {
      agent {
        docker {
          image 'maven:latest'
          args '-v /root/.m2:/root/.m2'
        }
      }
      steps {
        echo 'At this point  we should use maven or a tool to compile'
        sh 'mvn -B -DskipTests clean package'
      }
    }

  }
}
