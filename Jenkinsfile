pipeline {
  agent any
  stages {
    stage('Code download') {
      steps {
        echo 'Start cloning repository to execute pipeline'
        git(url: 'https://github.com/kikovalle/pipelinestests', branch: 'master', changelog: true)
      }
    }

    stage('Compilar') {
      steps {
        echo 'At this point  we should use maven or a tool to compile'
      }
    }

  }
}