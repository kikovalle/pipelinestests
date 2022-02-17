def code
node("maven") {
  stage("Parameters test step") {
    properties([
      parameters([
        choice(
          choices: ["main", "test"],
          name: "SOURCEBRANCH"
        ),
        booleanParam(
          defaultValue: false,
          description: "Generate a tag after successfull build",
          name: "TAGAFTERBUILD"
        ),
        string(
          defaultValue: "Jenkins Admin",
          name: "USERNAME",
          trim: true
        )
      ])
    ])
  }
  stage("Greet user and log choice in job") {
    echo "User requestion release: ${params.USERNAME}"
    echo "Selected branch to get source: ${params.SOURCEBRANCH}"
    if (params.TAGAFTERBUILD) {
      echo "User wants to generate a tag with release"
    } else {
      echo "Tag creation not needed due to user selection"
    }
  }
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
