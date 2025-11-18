pipeline {
    agent any
    tools {
        maven "M3"
    }
    
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/GnanapragasamRoyston/HelloWorldMaven.git',
                    branch: 'master'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
    post {
    success {
        script {
            def tag = "v-2.${BUILD_NUMBER}"
            echo "Build réussi ! Création du tag ${tag} et push sur GitHub..."

            // Configuration git
            sh '''
                git config user.name "GnanapragasamRoyston"
                git config user.email "royston0912@gmail.com"
            '''

            // Création du tag
            sh "git tag -a ${tag} -m 'Build Jenkins #${BUILD_NUMBER} - SUCCESS'"

            // Push du tag avec le credential
            withCredentials([usernamePassword(
                credentialsId: 'tokenJenkins',
                usernameVariable: 'GIT_USER',
                passwordVariable: 'GIT_TOKEN'
            )]) {
                // Utilise les variables d'environnement sans interpolation
                sh 'git push https://\\${GIT_USER}:\\${GIT_TOKEN}@github.com/GnanapragasamRoyston/HelloWorldMaven.git ${tag}'
            }
            echo "Tag ${tag} créé et poussé avec succès !"
        }
    }
    failure {
        echo "Build échoué → aucun tag créé"
    }
}


}
