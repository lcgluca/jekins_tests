def sfEnvProps
def jenkinsProps
def slackResponse
def args_1
def args_2
def creds_1
def creds_2

def colorMap = [
        'SUCCESS': 'good',
        'FAILURE': 'danger',
]

pipeline {

    agent {
        docker {
            image 'lcgluca/luca_public_repo:sfdx'
            registryCredentialsId 'luca_dockerHub'
            registryUrl 'https://docker.io'
        }
    }

    environment {
        JWT_KEY_FILE = credentials('salesforce_certificate')
        BRANCH_NAME = 'projectEbury'
        SLACK_CHANNEL = 'luca_test_channel'
        SFDC_HOST = 'https://login.salesforce.com'
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    sfEnvProps = readProperties(file: "jenkinsJobs/gouveatechservices.properties",
                            text: 'other=Override')

                    jenkinsProps = readProperties(file: 'jenkinsJobs/jenkinsPaths.properties',
                            text: 'other=Override')

                    args_1 = [
                        'duration' : '10',
                        'orgName'  : 'SC_NUMBER_1',
                        'username' : sfEnvProps.SF_USERNAME,
                        'jwtFile'  : JWT_KEY_FILE,
                        'consumerKey' : sfEnvProps.SF_CONNECTED_APP_CONSUMER_KEY
                    ]

                    args_2 = [
                        'duration' : '10',
                        'orgName'  : 'SC_NUMBER_2',
                        'username' : sfEnvProps.SF_USERNAME,
                        'jwtFile'  : JWT_KEY_FILE,
                        'consumerKey' : sfEnvProps.SF_CONNECTED_APP_CONSUMER_KEY
                    ]

                    sh(label: 'Log in to Prod', 
                        script: "sfdx auth:jwt:grant \
                                --clientid ${sfEnvProps.SF_CONNECTED_APP_CONSUMER_KEY} \
                                --username ${sfEnvProps.SF_USERNAME} \
                                --jwtkeyfile ${JWT_KEY_FILE} \
                                --setdefaultdevhubusername \
                                --instanceurl ${SFDC_HOST} --setalias GouveaTechServices")

                    println('certificate worked')
                }
            }
        }

        /* stage('Creation of SCT_ORGS') {
            parallel{
                stage('Scratch 1') {
                    steps {
                        script{
                            creds_1 = myTest(args_1)
                        }
                    }
                }
                stage('Scratch 2') { 
                    steps {
                        script{
                            sleep(100)
                            creds_2 = myTest(args_2)
                        }
                    }
                }
            }
            post {
                always {
                    println('END OF JOB WITH ' +currentBuild.currentResult)
                    println(creds_1)
                    println(creds_2)
                }
            }
        } */
    }
}