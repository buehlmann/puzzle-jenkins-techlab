@Library('puzzle-jenkins-techlab-libraries') _

properties([
        buildDiscarder(logRotator(numToKeepStr: '5')),
        pipelineTriggers([
                pollSCM('H/5 * * * *')
        ])
])

try {
    timestamps() {
        timeout(time: 10, unit: 'MINUTES') {
            env.ARTIFACT = "${env.JOB_NAME.split('/')[0]}-hello"
            env.REPO_URL = 'https://artifactory.puzzle.ch/artifactory/ext-release-local'
            node(env.JOB_NAME.split('/')[0]) {
                stage('Build') {
                    try {
                        withCredentials([file(credentialsId: 'm2_settings', variable: 'M2_SETTINGS'), usernameColonPassword(credentialsId: 'jenkins-artifactory', variable: 'ARTIFACTORY'), file(credentialsId: 'known_hosts', variable: 'KNOWN_HOSTS')]) {  // Credentials Binding Plugin
                            withEnv(["JAVA_HOME=${tool 'jdk8'}", "PATH+MAVEN=${tool 'maven35'}/bin:${env.JAVA_HOME}/bin"]) {
                                checkout scm
                                sh 'mvn -B -V -U -e clean verify -Dsurefire.useFile=false'
                                sh "mvn -s '${M2_SETTINGS}' -B deploy:deploy-file -DrepositoryId='puzzle-releases' -Durl='${REPO_URL}' -DgroupId='com.puzzleitc.jenkins-techlab' -DartifactId='${ARTIFACT}' -Dversion='1.0' -Dpackaging='jar' -Dfile=`echo target/*.jar`"
                            }

                            sshagent(['testserver']) {  // SSH Agent Plugin
                                sh "ls -l target"
                                sh "ssh -o UserKnownHostsFile='${KNOWN_HOSTS}' -p 2222 richard@testserver.vcap.me 'curl -O -u \'${ARTIFACTORY}\' ${REPO_URL}/com/puzzleitc/jenkins-techlab/${ARTIFACT}/1.0/${ARTIFACT}-1.0.jar && ls -l'"
                            }
                        }

                        archiveArtifacts 'target/*.?ar'
                    } finally {
                        junit 'target/**/*.xml'  // JUnit Plugin
                    }
                }
            }
        }
    }
} finally {
    notifyPuzzleChat()
}
