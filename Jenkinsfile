node{     
        try
        {
	stage 'Checkout'
  	checkout scm
		seeenv()
		notifyBuild(currentBuild.result)
    
  	stage 'Build_Backend_Code'
	echo "Running: Build_Backend_Code"
	sh "pwd"
		seeenv()
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

def seeenv() {
	for(e in env){
        echo e + " is " + ${e}
	}
}
	
def notifyBuild(String buildStatus = 'STARTED') {
  buildStatus =  buildStatus ?: 'SUCCESSFUL'
	
	def build_number = "${env.BUILD_NUMBER}"
        def job = env.JOB_NAME.split('/')
        def job_name = job[0]
        def branch_name = job[1]
  	def git_branch_name = branch_name.replaceAll("%2F","/")
  	def url_branch_name = git_branch_name.replaceAll("/","%252F")
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
  }
		println "Continuous Integration pipeline on ${url_branch_name}: ${buildStatus}\ncheck ${env.BUILD_URL}"
                sh "git log --after 1.days.ago|egrep -io '[a-z0-9\\-\\._@]++\\.[a-z0-9]{1,4}'|head -1 >lastAuthor"
  		def lines = readFile("lastAuthor")
                println "Email notifications will be send to : ${lines}"
	mail bcc: '', body: "${summary}", cc: 'abhishek.tamrakar@reancloud.com', charset: 'UTF-8', mimeType: 'text/plain', subject: "${subject}", to: "${lines}"
}
