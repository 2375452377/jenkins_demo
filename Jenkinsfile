pipeline {
    agent { docker 'python:3.5.1' }
    triggers{
        pollSCM('* * * * *')
    }
    stages {
        stage('build') {
            steps {
                sh 'python --version'
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                sh 'chmod 555 ./flakey-deploy.sh ./health-check.sh'
            }
        }

        stage('deploy') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(3) {
                        sh './flakey-deploy.sh'
                    }
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                }
            }
        }
    }
}