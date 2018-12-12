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
                        def results = readFile "${env.WORKSPACE}/target/surefire-reports/training.AppTest.txt"
                        echo results
                    }

                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'env.DOLL',
                        from: 'andreas@berrou.de',
                        body: '${FILE,path="target/surefire-reports/AppTest.txt.xml"}',
                        attachmentsPattern: 'target/surefire-reports/*.xml'
                    )

                    sh '''
                         echo 
                     '''
               }
            }
        }
    }
}
