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

                    echo 'Building anotherJob and getting the log'
                    script {
                        def version = readFile "${env.WORKSPACE}/target/surefire-reports/training.AppTest.txt"
                    }

                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'test',
                        from: 'andreas@berrou.de',
                        body: "${currentBuild.rawBuild.getLog(100)}",
                        attachmentsPattern: 'target/surefire-reports/*.xml'
                    )
               }
            }
        }
    }
}
