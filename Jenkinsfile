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
                        def version = readFile "${env.WORKSPACE}/target/surefire-reports/training.AppTest.txt"
                        echo version
                        ${version} = version
                    }

                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: '''${version}''',
                        attachmentsPattern: 'target/surefire-reports/*.xml'
                    )
               }
            }
        }
    }
}
