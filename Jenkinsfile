build_number = "${env.BUILD_NUMBER}"
job = env.JOB_NAME.split('/')
job_name = job[0]
branch_name = job[1]
git_branch_name = branch_name.replaceAll("%2F","/")
url_branch_name = git_branch_name.replaceAll("/","%252F")

node{   
	try 
	{
	stage 'Checkout'
  	checkout scm 
		
  	stage 'Build_Backend_Code'
	echo "Running: Build_Backend_Code"
//			def ret = sh(script: 'uname', returnStdout: true)
/*		def reti = sh(script: 'unamer', returnStatus: true)
		echo "ret=${ret}"
		if(reti > 0) {
		notifyBuild("FAILED")
		} */
		sh "uname"
		//step([$class: 'GitHubCommitStatusSetter', errorHandlers: [[$class: 'ChangingBuildStatusErrorHandler', result: 'FAILURE']], statusResultSource: [$class: 'ConditionalStatusResultSource', results: [[$class: 'BetterThanOrEqualBuildResult', message: 'SUCCESSFUL', result: 'SUCCESS', state: 'SUCCESS']]]])
}
        catch(Exception err)
        { 
		stage('Email') {
			notifyBuild("FAILED","${err}")
			//sh "exit 1"
		  		}
		throw err
	} 
	notifyBuild(currentBuild.result,"OKAY")
}

def sendMail(String buildStat,String errr) {
	def subject = "${buildStat}: Job '${job} [${build_number}]'"
	def summary = "${subject} with ${errr}\n(${env.BUILD_URL})"
		println "Continuous Integration pipeline on ${url_branch_name}: ${buildStat}\ncheck ${env.BUILD_URL}"
                sh "git log --after 1.days.ago|egrep -io '[a-z0-9\\-\\._@]++\\.[a-z0-9]{1,4}'|head -1 >lastAuthor"
  		def lines = readFile("lastAuthor")
                println "Email notifications will be send to : ${lines}"
	def emailStr = "nikhil.kapure@teradata.com"
	def regexStr = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}$/
if (emailStr.matches(regexStr)){
  // If we arrive here then the emailStr is a correctly formatted email string
	println "${emailStr} is valid email"
	} else {
  // If we arrive here then the email address is not correctly formatted and needs to be handled somehow.
	println "${emailStr} is not valid email"
} 
}
	
def notifyBuild(String buildStatus = 'STARTED',String thiserr) {
  buildStatus =  buildStatus ?: 'SUCCESSFUL'
	
	def colorName = 'RED'
  	def colorCode = '#FF0000'
	
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
	 /* step([$class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource',
        context: 'SUCCESS Report'],
        statusResultSource: [$class: 'ConditionalStatusResultSource',
        results: [[$class: 'AnyBuildResult',
        message: 'The Build was SUCCESSFUL',
        state: '${buildStatus}']]]])
        echo "status set to ${buildStatus}." */
	  sendMail("SUCCESSFUL","OKAY")
  } else if (buildStatus == 'FAILED') {
    color = 'RED'
    colorCode = '#FF0000'
	/*  step([$class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource',
        context: 'FAILED Report'],
        statusResultSource: [$class: 'ConditionalStatusResultSource',
        results: [[$class: 'AnyBuildResult',
        message: 'The Build was FAILED',
        state: '${buildStatus}']]]])
        echo "status set to ${buildStatus}." */
	  sendMail("FAILED","${thiserr}")
	    }
}
