properties([parameters([string(defaultValue: 'puzzle', description: 'The company the pipeline runs in', name: 'company_parameter')])])

node {
    stage('Build') {
        sh "echo \"Running ${env.BUILD_ID} on ${env.JENKINS_URL}\" in company ${params.company_parameter}"
    }
}
