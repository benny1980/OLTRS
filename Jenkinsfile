
node {

    stage('Checkout') {
        echo 'Checking out OLTRS repository'

        git branch: 'master',
            url: 'https://github.com/archanareddyse/OLTRS.git'
    }

    stage('Build and Test') {
        echo 'Building OLTRS using Maven'

        bat 'mvn clean package'
    }

}
