build_number = "${env.BUILD_NUMBER}"
job = env.JOB_NAME.split('/')
job_name = job[0]
branch_name = job[1]
git_branch_name = branch_name.replaceAll("%2F","/")
url_branch_name = git_branch_name.replaceAll("/","%252F")

node{   
	notifyBuild("STARTED")
	try 
	{
	stage 'Checkout'
  	checkout scm 
		
  	stage 'Build_Backend_Code'
	echo "Running: Build_Backend_Code"
	sh "aws run instance"	
}
        catch(Exception e)
        { 
		notifyBuild(currentBuild.result)
		throw e
				}
}
	
def notifyBuild(String buildStatus = 'STARTED') {
  buildStatus =  buildStatus ?: 'SUCCESSFUL'
	
	def colorName = 'RED'
  	def colorCode = '#FF0000'
	def subject = "${buildStatus}: Job '${job} [${build_number}]'"
  	def summary = "${subject} (${env.BUILD_URL})"

  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
	  step([$class: 'GitHubCommitStatusSetter',
        contextSource: [$class: 'ManuallyEnteredCommitContextSource',
        context: '${buildStatus} Report'],
        statusResultSource: [$class: 'ConditionalStatusResultSource',
        results: [[$class: 'AnyBuildResult',
        message: 'The Build was ${buildStatus}',
        state: '${buildStatus}']]]])
        echo "status set to ${buildStatus}."
  }
		println "Continuous Integration pipeline on ${url_branch_name}: ${buildStatus}\ncheck ${env.BUILD_URL}"
                sh "git log --after 1.days.ago|egrep -io '[a-z0-9\\-\\._@]++\\.[a-z0-9]{1,4}'|head -1 >lastAuthor"
  		def lines = readFile("lastAuthor")
                println "Email notifications will be send to : ${lines}"
	mail bcc: '', body: "${summary}", cc: 'atamrakar@localhost', charset: 'UTF-8', mimeType: 'text/plain', subject: "${subject}", to: "atamrakar@localhost"
}
