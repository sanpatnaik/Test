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
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
/*    stage('GIT Checkout')
	{   

                scmInfo = checkout([$class: 'GitSCM', branches: [[name: 'master']],userRemoteConfigs: [[credentialsId: 'sforce_jenkins_service', url: "http://thpgithub/salesforce/Commercial_New_Sales.git"]]])

                println 'GIT Checkout Successful'
				        println "${scmInfo.GIT_COMMIT}"

    }*/
	
    stage('Authenticate CI and Validate')
    {   
	
        withCredentials([file(credentialsId: 'SALESFORCE_PRIVATE_KEY', variable: 'JWT_Secret_CRT'), string(credentialsId: 'CONNECTED_APP_CONSUMER_KEY_DH', variable: 'cKey'), string(credentialsId: 'CD_USERNAME', variable: 'uName'),string(credentialsId: 'SFDC_HOST_DH_TEST', variable: 'hName')])
                {
                    echo "Client id ${cKey}"
					echo "UserName is ${uName}"
					echo "Instance Url is ${hName}"
					rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${cKey} --username ${uName} --jwtkeyfile \"${JWT_Secret_CRT}\" --instanceurl ${hName}"
                         if (rc != 0) 
						{ 
                          error 'org authorization failed' 
                        }
				    rc = sh returnStdout: true, script: "sfdx force:source:deploy -p force-app/main/default -u ${uName} -c"
			                  
				}
    }
}
