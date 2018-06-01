pipeline {
    triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'ref', value: '$.ref']
     ],
     causeString: 'Triggered on $ref',
     regexpFilterExpression: 'Jpetstore',
     regexpFilterText: 'Jpetstore',
     printContributedVariables: true,
     printPostContent: true
    )
  }
    agent any
    environment {
        SNOW_AUTH = credentials('snow_auth')
        SNOW_URL = "${env.SNOW_URL}"
    }
    stages {
        stage('Pre Checks') {
            steps {
                echo 'Pre Check...'
                echo 'Pre Check...COMPLETED'
               }
            }
         stage('Build') {
            steps {
                sh "/usr/bin/mvn clean install -Dskiptest"
                echo 'Build...COMPLETED'
               }
            }
        stage('Verify') {
            post {
                failure {
                    echo "RESULT: FAILURE"
                    sh "curl -v -H \"Accept: application/json\" -H \"Content-Type: application/json\" -X POST --data '{\"short_description\":\"Test incident creation through REST\", \"comments\":\"These are my comments - FAILURE\"}' -u \"$SNOW_AUTH\" \"https://${SNOW_URL}.service-now.com/api/now/v1/table/incident\""
                }
                fixed {
                    echo "RESULT: Fixed"
                }
                regression {
                    sh "curl -v -H \"Accept: application/json\" -H \"Content-Type: application/json\" -X POST --data '{\"short_description\":\"Test incident creation through REST\", \"comments\":\"These are my comments - REGRESSION\"}' -u \"$SNOW_AUTH\" \"https://${SNOW_URL}.service-now.com/api/now/v1/table/incident\""
                }
                success {
                    echo "RESULT: SUCCESS"
                }
            }
            steps {
                echo 'Testing..'
                sh "/usr/bin/mvn test"
            }
        }
    }
}
