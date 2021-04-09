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
        stage('Hello') {
            when {
                expression {
                    env.BRANCH_NAME == 'master'
                }
            }
            steps {
                echo 'Hello World'
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
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                withCredentials([
                    usernamePassword(credentials:'server-credentials', usernameVariable: USER, passwordVariable: PASSWORD)
                ]){
                    echo "some script ${USER} ${PASSWORD}"
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
