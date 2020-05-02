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

        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('run project') {
            steps {
                sh 'java -jar target/*.jar &'
            }
        }
    }
}