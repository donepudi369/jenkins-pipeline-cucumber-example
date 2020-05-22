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
        stage('copy cucumber'){
          steps {
              //script { 
                  //sh "cp /var/jenkins_home/jobs/MCS-Dev/jobs/Test_Jobs/jobs/cucumber-reports/builds/15/cucumber-html-reports/${BUILD_NUMBER}/cucumber-html-reports/* ${workspace}"
              //}
            dir("/var/jenkins_home/jobs/MCS-Dev/jobs/Test_Jobs/jobs/cucumber-reports/builds/${BUILD_NUMBER}/cucumber-html-reports/") {
              sh "pwd"
              sh "ls -la"
              sh "cp * ${workspace}/cucumber-html-reports"
            }
          }
        }
        stage ('Junit') {
          steps {
            archive "target/**/*"
            junit 'target/surefire-reports/*.xml'
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
