def gv
pipeline {
    agent any
    environment {
        NEW_VERSION = '1.1.0'
        SERVER_CREDENTIALS = credentials('jason-github')
    }
    parameters {
        string(name: 'VERSION', defaultValue: '1.0.0', description:'version to deploy on prod')
        booleanParam(name: 'executeTests', defaultValue: false, description:'')
    }
    tools {
        maven 'Maven'
        gradle 'Gradle'
        jdk 'JDK'
    }
    stages {
        stage('initScript') {
            steps {
                script {
                    // def newname = "John"
                    gv = load "script.groovy"
                }
            }
        }
        stage('Hello') {
            when {
                expression {
                    env.BRANCH_NAME == 'main'
                }
            }
            steps {
                echo 'Hello World'
                script {
                    gv.buildApp()
                }
            }
        }
        stage('Build') {
            when {
                expression {
                    params.executeTests == true
                }
            }
            steps {
                echo 'Building'
                echo "building version ${NEW_VERSION}"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying version ${params.VERSION}"
                echo "deploying with ${SERVER_CREDENTIALS}"
                echo "${SERVER_CREDENTIALS}"
                //withCredentials([usernamePassword(credentialsId: 'jason-github', usernameVariable: USER, passwordVariable: PSW)]) {
                  //echo "username is $USER password is $PSW"
                //}
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                withCredentials([usernamePassword(credentialsId: 'jason-github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                  // available as an env variable, but will be masked if you try to print it out any which way
                  // note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
                  sh 'echo $PASSWORD'
                  // also available as a Groovy variable
                  echo USERNAME
                  // or inside double quotes for string interpolation
                  echo "username is $USERNAME"
                }
                script {
                    gv.testApp()
                }
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}
