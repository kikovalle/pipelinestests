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
  
  stage("Checkout code") {
    git branch: "${params.SOURCEBRANCH}" , url: "https://github.com/kikovalle/pipelinestests"
  }
  
  stage("List folder contents") {
    sh "ls -lorth"
  }
  
  stage("Testing shared library") {
    sayHello params.USERNAME
    echo 'The value of foo is : ' + GlobalVars.defaultDeveloper 
    if (GlobalVars.defaultDeveloper  == params.USERNAME) {
      echo "User launching the job is the default developer"
    } else {
      echo "User is not default developer"
    }
  }

  stage("Compile") {
    withMaven( maven : 'maven' ) {
      sh 'mvn -B -DskipTests clean package'
    }
  }
  

  
  def buildStagesList = []
  stage("Prepare parallel stage") {
    def parallelTest = [:]
    parallelTest.put("test1", {
      stage("Test scripted parallel stage 1") {
          println("Executing test1 Stage 1 Parallel that makes a sleep 10 and reutnrStatus true")
          sh(script:'sleep 10', returnStatus:true)
        }
    })
    buildStagesList.add(parallelTest)
    
    parallelTest = [:]
    parallelTest.put("test2", {
      stage("Test scripted parallel stage 2") {
          println("Executing test2 Stage 2 Parallel  that makes a sleep 15 and reutnrStatus true")
          sh(script:'sleep 15', returnStatus:true)
        }
    })
    buildStagesList.add(parallelTest)
    echo "List of parallel stages ready"
  }
  
  stage("Parallel stage") {
    for(builds in buildStagesList) {
      parallel(builds)
    }
  }
  
  stage("End") {
    echo "Everything seems to work fine!"
  }
}
