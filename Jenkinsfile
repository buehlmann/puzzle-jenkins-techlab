pipeline {
    agent { label env.JOB_NAME.split('/')[0] }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    triggers {
        pollSCM('H/5 * * * *')
        cron('@midnight')
    }
    stages {
        stage('Build') {
            steps {
                withEnv(["JAVA_HOME=${tool 'jdk8_oracle'}", "PATH+MAVEN=${tool 'maven35'}/bin:${env.JAVA_HOME}/bin"]) {
                    sh 'mvn -B -V -U -e clean verify -Dsurefire.useFile=false'
                    archiveArtifacts 'target/*.?ar'
                }
            }
            post {
                always {
                    junit 'target/**/*.xml'  // Requires JUnit plugin
                }
            }
        }
    }
    post {
        success {
            rocketSend avatar: 'https://chat.puzzle.ch/emoji-custom/success.png', channel: 'jenkins-techlab', message: "Build success - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", rawMessage: true
        }
        unstable {
            rocketSend avatar: 'https://chat.puzzle.ch/emoji-custom/unstable.png', channel: 'jenkins-techlab', message: "Build unstable - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", rawMessage: true
        }
        failure {
            rocketSend avatar: 'https://chat.puzzle.ch/emoji-custom/failure.png', channel: 'jenkins-techlab', message: "Build failure - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", rawMessage: true
        }
    }
}
