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
                        env.put = ("results", results)
                    }

                    emailext(
                        to: 'andreas@berrou.de',
                        subject: 'env.DOLL',
                        from: 'andreas@berrou.de',
                        body: '$DOLL',
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
