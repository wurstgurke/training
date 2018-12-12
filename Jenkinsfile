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
                         find .                         
                     '''
                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: "${currentBuild.result}",
                        attachmentsPattern: './target/surefire-reports/TEST-training.AppTest.xml'
                    )
               }
            }
        }
    }
}
