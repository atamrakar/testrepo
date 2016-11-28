node {
            def causes = currentBuild.rawBuild.getCauses()
            try {
            // run tests in the same workspace that the project was built
            sh 'uname -a'
                        echo "cause: ${causes}"
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
