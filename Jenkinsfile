node {
              wrap([$class: 'BuildUser']) {
    def user = env.BUILD_USER_ID
  }

            
            try {
            // run tests in the same workspace that the project was built
                                    sh 'uname -a'            
                        echo "user: ${user}"
        } catch (e) {
            // if any exception occurs, mark the build as failed
            currentBuild.result = 'FAILURE'
            throw e
        } finally {
            // perform workspace cleanup only if the build have passed
            // if the build has failed, the workspace will be kept
            step([$class: 'WsCleanup', cleanWhenFailure: true])
        }
}
