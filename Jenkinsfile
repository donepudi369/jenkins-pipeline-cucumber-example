pipeline{
    agent {
      label 'maven'
    }
    stages {
        stage ('Compile Stage') {
            steps {
              sh 'mvn clean install'
            }
        }
    stage ('Test Stage') {
            steps {
               sh 'mvn test'
            }
        }

        stage ('Cucumber Reports') {
            steps {
                cucumber buildStatus: "UNSTABLE",
                    fileIncludePattern: "**/cucumber.json",
                    jsonReportDirectory: 'target'
            }
        }
    }
}


pipeline{
    agent {
      label 'maven'
    }
    stages {
        stage ('Compile Stage') {
            steps {
              sh 'mvn clean install'
            }
        }
    stage ('Test Stage') {
            steps {
               sh 'mvn test'
            }
        }

        stage ('Cucumber Reports') {
            steps {
                cucumber buildStatus: "UNSTABLE",
                    fileIncludePattern: "**/cucumber.json",
                    jsonReportDirectory: 'target'
            }
        }
        stage ('Junit') {
          steps {
            junit 'target/surefire-reports/**/*.xml'
          }
        }
    }
    post{
      always {        
        emailext(
          to: 'surendra.donepudi@pdisoftware.com',
          replyTo: 'surendra.donepudi@pdisoftware.com',
          subject: "'${JOB_NAME}' (${BUILD_NUMBER}) ${currentBuild.currentResult}",
          mimeType: 'text/html',
          attachLog: true,
          attachmentsPattern: '**/${JOB_BASE_NAME}-${BUILD_NUMBER}*',
          body: '${SCRIPT, template="groovy-html.template"}'
          )
      }        
    }
}
