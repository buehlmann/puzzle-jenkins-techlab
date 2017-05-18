pipeline {
    agent any
    stages {
        stage('script') {
            steps {
                script {
                    def pipelineType = 'declarative'
                    echo "yeah we executed a script within the ${pipelineType} pipeline"
                }
            }
        }
    }
}
