node {
    def dockerImage = null

    stage('Prepare Docker Environment') {
        dockerImage = docker.image('node:16-buster-slim')
        dockerImage.run('-p 3000:3000')
    }

    try {
        stage('Build') {
            dockerImage.inside {
                sh 'npm cache clean --force'
                sh 'npm install'
            }
        }

        stage('Test') {
            dockerImage.inside {
                sh './jenkins/scripts/test.sh'
            }
        }
    } finally {
        stage('Clean Up') {
            dockerImage.stop()
        }
    }
}
