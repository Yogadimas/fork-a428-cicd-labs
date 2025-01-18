node {
    def dockerImage

    stage('Prepare Docker Environment') {
        dockerImage = docker.image('node:16-buster-slim')
        dockerImage.run('-p 3000:3000')
    }

    stage('Build') {
        dockerImage.inside {
            sh 'npm cache clean --force'
            checkout scm
            sh 'npm install'
        }
    }

    stage('Test') {
        dockerImage.inside {
            sh './jenkins/scripts/test.sh'
        }
    }

    stage('Manual Approval') {
        dockerImage.inside {
            sh './jenkins/scripts/deliver.sh' 
            input message: 'Lanjutkan ke tahap Deploy?' 
            sh './jenkins/scripts/kill.sh'  
        }
    }

    stage('Deploy') {
        dockerImage.inside {
            sh './jenkins/scripts/deliver.sh' 
            sleep time: 1, unit: 'MINUTES'
            sh './jenkins/scripts/kill.sh' 
        }
    }

}
