pipeline {
	agent any
	stages {
		stage('Build') {
			steps {
				script {
					def company = 'puzzle'
					echo 'join the ${company}'
					echo "join the ${company}"
					echo '''join the ${company}'''
					echo """join the ${company}"""
				}
			}
		}
	}
}
