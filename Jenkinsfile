#!groovy

import groovy.json.JsonSlurperClassic

node {

    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
    def SF_USERNAME=env.SF_USERNAME
    def SERVER_KEY_CREDENTALS_ID=env.SERVER_KEY_CREDENTALS_ID
    def TEST_LEVEL=env.TEST_LEVEL
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL

    def toolbelt = tool 'toolbelt'


    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }


    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------
        
    withCredentials([file(credentialsId: SERVER_KEY_CREDENTALS_ID, variable: 'server_key_file')]) {

        // -------------------------------------------------------------------------
        // Authorize the Dev Hub org with JWT key and give it an alias.
        // -------------------------------------------------------------------------

        stage('Authorize Org') {
            if(isUnix()){
                rc = command '${toolbelt}/sfdx auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile ${server_key_file} --setalias testOrg'
            } else{
                rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile \"${server_key_file}\" --setalias testOrg"
            }
            if (rc != 0) {
                error 'Salesforce authorization failed.'
            }
        }


        // -------------------------------------------------------------------------
        // Push source to test org.
        // -------------------------------------------------------------------------

        //stage('Push To Test Org') {
        //    rc = command "${toolbelt}/sfdx force:source:push --targetusername testOrg"
        //    if (rc != 0) {
        //        error 'Salesforce push to test org failed.'
        //    }
        //}


        // -------------------------------------------------------------------------
        // Run unit tests in test org.
        // -------------------------------------------------------------------------

        //stage('Run Tests In Test Org') {
        //    rc = command "${toolbelt}/sfdx force:apex:test:run --targetusername testOrg --wait 10 --resultformat tap --codecoverage --testlevel ${TEST_LEVEL}"
        //    if (rc != 0) {
        //       error 'Salesforce unit test run in test org failed.'
        //    }
        // }
    }
}
