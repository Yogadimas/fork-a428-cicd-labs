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

    stage('Deploy') {
        dockerImage.inside {
            sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
            sh './jenkins/scripts/kill.sh' 
        }
    }

}
