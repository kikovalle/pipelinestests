@Library("kiko- jenkinstest-library") _

import com.github.kikovalle.jenkins.GlobalVars
import com.github.kikovalle.jenkins.HelmDeployer

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
  
  parallel (
    Greeting: {
      echo "User requestion release: ${params.USERNAME}"
      echo "Selected branch to get source: ${params.SOURCEBRANCH}"
      if (params.TAGAFTERBUILD) {
        echo "User wants to generate a tag with release"
      } else {
        echo "Tag creation not needed due to user selection"
      }
    },
    CheckoutCode: {
      git branch: "${params.SOURCEBRANCH}" , url: "https://github.com/kikovalle/pipelinestests"
    }
  )
  
  stage("List folder contents") {
    sh "ls -lorth"
  }
  
  parallel (
    TestSharedLibrary: {
      sayHello params.USERNAME
      echo 'The value of foo is : ' + GlobalVars.defaultDeveloper 
      echo "We are in build number ${currentBuild.getNumber()}"
      if (GlobalVars.defaultDeveloper  == params.USERNAME) {
        echo "User launching the job is the default developer"
      } else {
        echo "User is not default developer"
      }
    },
    CompileWithMaven: {
      GlobalVars.mvn this, 'clean package'
    },
    TestWait1: {
      println("Executing test1 Stage 1 Parallel that makes a sleep 10 and reutnrStatus true")
      sh(script:'sleep 10', returnStatus:true)
    },
    TestWait2: {
      println("Executing test2 Stage 2 Parallel  that makes a sleep 15 and reutnrStatus true")
      sh(script:'sleep 15', returnStatus:true)
    }
  )
  
  node {
    stage("Preparing helm deploy example") {
      dir("examples/charts/") {
        echo "Proceed to download hello world example chart to deploy from any github example project as we are trying to test pipelines"
        git  branch: "${params.SOURCEBRANCH}" , url: "https://github.com/helm/examples"
      }
    }
    stage("Deploy with helm") {
      dir("examples/charts/") {
        HelmDeployer.deploy this, 'hello-world',  "${params.SOURCEBRANCH}"
      }
    }
  }
  
  
  
  
  stage("End") {
    echo "Everything seems to work fine!"
  }
}
