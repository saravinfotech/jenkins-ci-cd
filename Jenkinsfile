pipeline{
    agent any
    tools{
        maven "maven"
    }
    stages{
        stage("SCM Checkout"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/saravinfotech/jenkins-ci-cd.git']])
            }
        }
        stage("build"){
            steps{
                sh 'mvn clean install'
            }
        }
        stage("deploy WAR"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-pwd', path: '', url: 'http://localhost:9090/')], contextPath: 'jenkinsCiCd', war: '**/*.war'
            }
        }
    }
    post{
        always{
            emailext attachLog: true, body: '''<html>
    <body>
        <p>Build Status: ${BUILD_STATUS}</p>
        <p>Build Number: ${BUILD_NUMBER}</p>
        <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
    </body>
<html>''', mimeType: 'text/html', replyTo: 'saravinfotech@gmail.com', subject: 'Pipeline Status : ${BUILD_NUMBER}', to: 'saravinfotech@gmail.com'
        }
    }
}


//Stages
//SCM Checkout
//build
//deploy WAR
//Email