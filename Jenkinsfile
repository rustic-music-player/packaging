pipeline {
    agent none

    triggers {
        pollSCM('H/30 * * * *')
        upstream(upstreamProjects: 'rustic/daemon/master,rustic/web-app/master', threshold: hudson.model.Result.SUCCESS)
    }

    stages {
        stage('Package') {
            parallel {
                stage('Linux') {
                    agent any
                    steps {
                        copyArtifacts filter: 'rustic-linux-x86_64', projectName: '/rustic/daemon/master', target: 'linux'
                        fileOperations([fileRenameOperation(source: 'linux/rustic-linux-x86_64', destination: 'linux/rustic')])
                        copyArtifacts filter: 'rustic-web-client.zip', projectName: '/rustic/web-app/master'
                        unzip zipFile: 'rustic-web-client.zip', dir: 'linux/static'
                    }
                    post {
                        success {
                            zip zipFile: 'rustic-linux.zip', archive: true, dir: 'linux'
                        }
                    }
                }

                stage('Windows') {
                    agent any
                    steps {
                        copyArtifacts filter: 'rustic-win32-x86_64.exe', projectName: '/rustic/daemon/master', target: 'windows'
                        fileOperations([fileRenameOperation(source: 'windows/rustic-win32-x86_64.exe', destination: 'windows/rustic.exe')])
                        copyArtifacts filter: 'rustic-web-client.zip', projectName: '/rustic/web-app/master'
                        unzip zipFile: 'rustic-web-client.zip', dir: 'windows/static'
                    }
                    post {
                        success {
                            zip zipFile: 'rustic-windows.zip', archive: true, dir: 'windows'
                        }
                    }
                }

                stage('macOS') {
                    agent any
                    steps {
                        copyArtifacts filter: 'rustic-osx-x86_64', projectName: '/rustic/daemon/master', target: 'macos'
                        fileOperations([fileRenameOperation(source: 'macos/rustic-osx-x86_64', destination: 'macos/rustic')])
                        copyArtifacts filter: 'rustic-web-client.zip', projectName: '/rustic/web-app/master'
                        unzip zipFile: 'rustic-web-client.zip', dir: 'macos/static'
                    }
                    post {
                        success {
                            zip zipFile: 'rustic-macos.zip', archive: true, dir: 'macos'
                        }
                    }
                }
            }
        }
    }
}
