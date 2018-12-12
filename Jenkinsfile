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
                        def bRun = build 'anotherJob' 
                        echo 'last 100 lines of BuildB'
                        for(String line : bRun.getRawBuild().getLog(100)){
                            echo line
                        }
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
