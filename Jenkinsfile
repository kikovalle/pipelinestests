@Library("kiko- jenkinstest-library") _

import com.github.kikovalle.jenkins.GlobalVars

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
  stage ("Greets and checkout code") {
    parallel(
      stage("Testing shared library") {
        sayHello params.USERNAME
        echo 'The value of foo is : ' + GlobalVars.defaultDeveloper 
        if (GlobalVars.defaultDeveloper  == params.USERNAME) {
          echo "User launching the job is the default developer"
        } else {
          echo "User is not default developer"
        }
      }, stage("Checkout code") {
        checkout scm
      }
    )
  }
  
  
  stage("List workspace code and load groovy script in repo") {
    parallel 'list': {
      stage("List folder contents") {
        sh "ls -lorth"
      }, 'loadlibrary': {
        stage("Load") {
          code = load "script-example.groovy"
        }
      }
    }
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
