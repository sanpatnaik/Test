#!groovy
import groovy.json.JsonSlurperClassic
node 
{
    def toolbelt = tool 'sfdx_tool'
	
    stage('PreStep: Clean Workspace')
	{
	
	 cleanWs()
	 
	 println 'WorkSpace Cleaned'
	 
	}
  
  stage('Checkout Branch') {
        checkout scm
    }
	
    stage('Authenticate CI and Validate')
    {   
	
        withCredentials([file(credentialsId: 'SALESFORCE_PRIVATE_KEY', variable: 'JWT_Secret_CRT'), string(credentialsId: 'CONNECTED_APP_CONSUMER_KEY_DH', variable: 'cKey'), string(credentialsId: 'CD_USERNAME', variable: 'uName'),string(credentialsId: 'SFDC_HOST_DH_TEST', variable: 'hName')])
                {
                    echo "Client id ${cKey}"
					echo "UserName is ${uName}"
					echo "Instance Url is ${hName}"
			rc1 = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:logout --targetusername ${uName} -p"
                         
					rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${cKey} --username ${uName} --jwtkeyfile \"${JWT_Secret_CRT}\" --instanceurl ${hName}"
                         if (rc != 0) 
						{ 
                          error 'org authorization failed' 
                        }
				    rc = sh returnStdout: true, script: "${toolbelt}/sfdx force:source:deploy -p force-app/main/default -u ${uName} -c"
			                  
				}
    }
}
