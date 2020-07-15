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
                        copyArtifacts filter: 'linux-x86_64/rustic', flatten: true, projectName: '/rustic/daemon/master', target: 'linux'
                        copyArtifacts filter: 'linux-x86_64/extensions/*', flatten: true, optional: true, projectName: '/rustic/daemon/master', target: 'linux/extensions'
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
                        copyArtifacts filter: 'win32-x86_64/rustic.exe', flatten: true, projectName: '/rustic/daemon/master', target: 'windows'
                        copyArtifacts filter: 'win32-x86_64/extensions/*', flatten: true, optional: true, projectName: '/rustic/daemon/master', target: 'windows/extensions'
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
                        copyArtifacts filter: 'osx-x86_64/rustic', flatten: true, projectName: '/rustic/daemon/master', target: 'macos'
                        copyArtifacts filter: 'osx-x86_64/extensions/*', flatten: true, optional: true, projectName: '/rustic/daemon/master', target: 'macos/extensions'
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
