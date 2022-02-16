def code
node("maven") {
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
    sh 'mvn -B -DskipTests clean package'
  }
  stage("End") {
    echo "Everything seems to work fine!"
  }
}
