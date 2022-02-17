def code
node("maven") {
  stage("Checkout code") {
    checkout scm
  }
  stage("Preload") {
    sh "ls -lorth"
  }
  stage("Load") {
    code = load "script-example.groovy"
  }
  stage("Exec") {
    code.example1()
  }
  stage("Compile") {
    withMaven( maven : 'maven' ) {
      sh 'mvn -B -DskipTests clean package'
    }
  }
  stage("End") {
    echo "Everything seems to work fine!"
  }
}
