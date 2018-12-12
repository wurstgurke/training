pipeline {
    agent {
        docker { image 'maven:3-alpine' }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Run tests with maven') {
            steps {
                 ansiColor('xterm') {
                     sh '''
                         mvn test
                     '''
                 }
            }
            post {
               always {
                    sh '''
                         pwd
                         find .                         
                     '''
                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: "${currentBuild.result}",
                        attachmentsPattern: '/var/jenkins_home/workspace/training/target/surefire-reports/*.xml'
                    )
               }
            }
        }
    }
}
