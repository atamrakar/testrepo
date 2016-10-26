node{

        def build_number = "${env.BUILD_NUMBER}"
        def job = env.JOB_NAME.split('/')
        def job_name = job[0]
        def branch_name = job[1]
  	def git_branch_name = branch_name.replaceAll("%2F","/")
  	def url_branch_name = git_branch_name.replaceAll("/","%252F")
        def error_url = "http://localhost:8080/job/${job_name}/job/${url_branch_name}/${build_number}/console"

        try
        {
	stage 'Checkout'
  	checkout scm
		notifyBuild(currentBuild.result)
    
  	stage 'Build_Backend_Code'
	echo "Running: Build_Backend_Code"
	sh "pwd"
		notifyBuild(currentBuild.result)
 	echo "test run coompleted"
}
        catch(e)
        { 
		notifyBuild(currentBuild.result)
		throw e
				} finally {
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
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
  }
	stage 'Email Notification'
                println "Continuous Integration pipeline at ${env.JOB_NAME}: ${buildStatus}"
                sh "git log --after 1.days.ago|egrep -io '[a-z0-9\\-\\._@]++\\.[a-z0-9]{1,4}'|head -1 >lastAuthor"
  		def lines = readFile("lastAuthor")
                println "Email notifications will be send to : ${lines}"
	mail bcc: '', body: "${summary}", cc: 'abhishek.tamrakar@reancloud.com', charset: 'UTF-8', from: '', mimeType: 'text/plain', replyTo: '', subject: "${subject}", to: "${lines}"
}
