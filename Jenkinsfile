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

                    script {
                        results = readFile "${env.WORKSPACE}/target/surefire-reports/training.AppTest.txt"
                    }

                    echo results

                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'Projekt X: nächtliches Testergebnis: $results',
                        from: 'andreas@berrou.de',
                        body: '${FILE,path="target/surefire-reports/training.AppTest.txt"}',
                        attachmentsPattern: 'target/surefire-reports/*.xml'
                    )
               }
            }
        }
    }
}
