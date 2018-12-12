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
                         printenv
                     '''
                 }
            }
            post {
               always {
                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: 
                            '''
                              There are ${currentBuild.result} 
                            ''',
                        attachmentsPattern: './cucumber/build/html/cucumber-html-reports/overview-features.html'
                    )
               }
            }
        }
    }
}
