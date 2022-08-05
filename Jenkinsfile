pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        
        stage('\033[34mHello\033[0m \033[33mcolorful\033[0m \033[35mworld!\033[0m') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/BartoszOciepka/spring-petclinic.git'

                // Run Maven on a Unix agent.
                sh "./mvnw -Dmaven.test.failure.ignore=true clean package"
                
                echo '\033[34mHello\033[0m \033[33mcolorful\033[0m \033[35mworld!\033[0m'

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                    emailext attachLog: true, body: 'test', compressLog: true, recipientProviders: [upstreamDevelopers(), buildUser()], subject: 'test'
                }
            }
        }
    }
}