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
		sh "aws run"
}
        catch(Exception err)
        { 
		stage('Email') {
			notifyBuild("FAILED")
			throw err
			//sh "exit 1"
		}
	} finally {
		echo "entering finally"
		notifyBuild(currentBuild.result)
	}
}

def sendMail(String buildStat) {
	def subject = "${buildStat}: Job '${job} [${build_number}]'"
	def summary = "${subject} (${env.BUILD_URL})"
		println "Continuous Integration pipeline on ${url_branch_name}: ${buildStat}\ncheck ${env.BUILD_URL}"
                sh "git log --after 1.days.ago|egrep -io '[a-z0-9\\-\\._@]++\\.[a-z0-9]{1,4}'|head -1 >lastAuthor"
  		def lines = readFile("lastAuthor")
                println "Email notifications will be send to : ${lines}"
	mail bcc: '', body: "${summary}", cc: 'atamrakar@localhost', charset: 'UTF-8', mimeType: 'text/plain', subject: "${subject}", to: "atamrakar@localhost"
}
	
def notifyBuild(String buildStatus = 'STARTED') {
  buildStatus =  buildStatus ?: 'SUCCESSFUL'
	
	def colorName = 'RED'
  	def colorCode = '#FF0000'
	
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
	  step([$class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource',
        context: 'SUCCESS Report'],
        statusResultSource: [$class: 'ConditionalStatusResultSource',
        results: [[$class: 'AnyBuildResult',
        message: 'The Build was SUCCESSFUL',
        state: '${buildStatus}']]]])
        echo "status set to ${buildStatus}."
	  sendMail("SUCCESSFUL")
  } else if (buildStatus == 'FAILED') {
    color = 'RED'
    colorCode = '#FF0000'
	  step([$class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource',
        context: 'FAILED Report'],
        statusResultSource: [$class: 'ConditionalStatusResultSource',
        results: [[$class: 'AnyBuildResult',
        message: 'The Build was FAILED',
        state: '${buildStatus}']]]])
        echo "status set to ${buildStatus}."
	  sendMail("FAILED")
	    }
}
