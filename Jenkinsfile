def gitBranch = "master"


pipeline {
    agent any
    tools { 
        maven 'mvn339' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage('Hello') {
         steps {
            script {

            echo "${currentBuild.buildCauses}" // same as currentBuild.getBuildCauses()
            echo "${currentBuild.getBuildCauses('hudson.model.Cause$UserCause')}"
            echo "${currentBuild.getBuildCauses('hudson.triggers.TimerTrigger$TimerTriggerCause')}"

            if ("${gitBranch}" == "master") {
            if(currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')){
            println('Cause: ' + currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause') + '\n calling devBuildAndDeploy()....')

            echo "user initiated"

             def userInput = input(id: 'userInput', message: 'Commit_id',
             parameters: [[$class: 'StringParameterDefinition', defaultValue: 'null', 
                description:'describing choices', name:'commitid']
             ])

            echo "$userInput";

            // steps.git url: "https://github.com/kd2dhamecha/website_springboot.git" , branch: "$userInput"

        checkout ( [$class: 'GitSCM',
        branches: [[name: "$userInput" ]],
        userRemoteConfigs: [[
            credentialsId: 'kd2dhamecha', 
            url: 'https://github.com/kd2dhamecha/website_springboot.git']]])

            }

            devBuildAndDeploy(gitBranch,userInput)
            }else{
              println('Cause: ' + currentBuild.getBuildCauses('hudson.model.Cause$BranchIndexingCause') + '\n calling devBuildAndDeploy()....')
              echo "auto initiated"
              devBuildAndDeploy(gitBranch)
            }
          }
         }
        }

        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('run project') {
            steps {
                sh 'java -jar target/*.jar'
            }
        }
    }
}

def devBuildAndDeploy(gitBranch,userInput){
    echo "$gitBranch"
    echo "$userInput"
}