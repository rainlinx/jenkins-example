pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        always {
            script { 
                def build = currentBuild // global variable in pipeline -> https://opensource.triology.de/jenkins/pipeline-syntax/globals#currentBuild
        
                def targetUrl = "http://192.168.8.147:8081/events"
                def buildUrl = build.absoluteUrl
                def projectName = build.projectName
                def buildNumber = build.number
                def buildStatus = build.currentResult
        
                httpRequest url: targetUrl, contentType: 'APPLICATION_JSON', httpMode: 'GET', responseHandle: 'NONE', timeout: 30, requestBody: """
                {
                    "name": "${projectName}",
                    "build": {
                        "full_url": "${buildUrl}",
                        "number": "${buildNumber}",
                        "phase": "FINISHED",
                        "status": "${buildStatus}"
                    }
                }
                """
            }
        }
    }
}
