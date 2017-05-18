pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    triggers {
        pollSCM('H/5 * * * *')
        cron('@midnight')
    }
    parameters {
        string(name: 'Greetings_to', defaultValue: 'Jenkins Techlab', description: 'Who to greet?')
    }
    stages {
        stage('Greeting') {
            steps {
                echo "Hello, ${params.Greetings_to}!"
            }
        }
    }
}
