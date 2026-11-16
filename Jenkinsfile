
node {

    def imageName = "bennyepaphra/oltrs:${env.BUILD_NUMBER}"

    try {

        stage('Checkout') {
            echo 'Checking out OLTRS source code'
            checkout scm
        }

        stage('Build and Test') {
            echo 'Building OLTRS using Maven'
            bat 'mvn clean package'
        }

        stage('Docker Build') {
            echo 'Creating Docker image'
            bat "docker build -t ${imageName} ."
        }

        stage('Docker Push') {
            echo 'Pushing image to Docker Hub'

            withCredentials([
                usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )
            ]) {

                bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'

                bat "docker push ${imageName}"

                bat 'docker logout'
            }
        }

        currentBuild.result = 'SUCCESS'

    } catch (Exception e) {

        currentBuild.result = 'FAILURE'

        echo "Pipeline failed: ${e}"

        throw e
    }
}
