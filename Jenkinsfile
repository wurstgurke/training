pipeline {
    agent any
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
                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: '''
                              <p>There are Tests failing!</p>
                              <p>Check reports and console output at &QUOT;<a href='$BUILD_URL'>$JOB_NAME [$BUILD_NUMBER]</a>&QUOT;</p>
                              ''',
                        attachmentsPattern: './cucumber/build/html/cucumber-html-reports/overview-features.html'
                    )
               }
            }
        }
    }
}
