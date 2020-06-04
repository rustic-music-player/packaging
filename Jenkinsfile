pipeline {
    agent none

    stages {
        stage('Package') {
            parallel {
                stage('Linux') {
                    agent any
                    steps {
                        copyArtifacts filter: '/rustic-linux-x86_64', projectName: '/rustic/daemon/master', target: 'linux'
                        copyArtifacts projectName: '/rustic/web-app/master', target: 'linux/static'
                    }
                    post {
                        success {
                            zip zipFile: '/rustic-linux.zip', archive: true, dir: 'linux'
                        }
                    }
                }

                stage('Windows') {
                    agent any
                    steps {
                        copyArtifacts filter: '/rustic-win32-x86_64.exe', projectName: '/rustic/daemon/master', target: 'windows'
                        copyArtifacts projectName: '/rustic/web-app/master', target: 'windows/static'
                    }
                    post {
                        success {
                            zip zipFile: '/rustic-windows.zip', archive: true, dir: 'windows'
                        }
                    }
                }

                stage('macOS') {
                    agent any
                    steps {
                        copyArtifacts filter: '/rustic-osx-x86_64', projectName: '/rustic/daemon/master', target: 'macos'
                        copyArtifacts projectName: '/rustic/web-app/master', target: 'macos/static'
                    }
                    post {
                        success {
                            zip zipFile: '/rustic-macos.zip', archive: true, dir: 'macos'
                        }
                    }
                }
            }
        }
    }
}
