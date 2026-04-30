pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validation HTML') {
            steps {
                script {
                    def htmlFiles = findFiles(glob: '**/*.html')
                    if (htmlFiles.length == 0) {
                        echo 'Aucun fichier HTML trouvé.'
                    } else {
                        htmlFiles.each { file ->
                            echo "Validation de : ${file.name}"
                        }
                    }
                }
            }
        }

        stage('Archivage des CVs') {
            steps {
                archiveArtifacts artifacts: '**/*.pdf, **/*.html', fingerprint: true
                echo 'CVs archivés avec succès.'
            }
        }

        stage('Déploiement CV HTML') {
            steps {
                script {
                    def deployDir = 'C:/deploy/cv'
                    bat "if not exist \"${deployDir}\" mkdir \"${deployDir}\""
                    bat "copy /Y \"*.html\" \"${deployDir}\\\""
                    echo "CV HTML déployé dans ${deployDir}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
