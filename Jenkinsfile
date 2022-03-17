pipeline {
    agent none
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }cd 
		stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --junit-xml output/results.xml sources/test_calc.py'
            }
            post {
                always {
					junit 'output/results.xml'
					archiveArtifacts artifacts: 'output/*.xml'
                }
            }
        }
    }
}