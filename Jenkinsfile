pipeline {

    agent {
        docker {
            image 'node:18-alpine'
        }
    }

    parameters {
        choice(
            name: 'ENTORNO',
            choices: ['staging', 'produccion'],
            description: 'Selecciona el entorno'
        )

        booleanParam(
            name: 'EJECUTAR_DEPLOY',
            defaultValue: false,
            description: '¿Ejecutar deploy?'
        )
    }

    environment {
        APP_NAME = "mi-proyecto"
    }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {

        stage('Instalar dependencias') {
            steps {
                sh 'npm install'
            }
        }

        stage('Pruebas') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        stage('Deploy') {
            when {
                expression { params.EJECUTAR_DEPLOY }
            }

            steps {
                echo "Desplegando ${env.APP_NAME} en ${params.ENTORNO}"
            }
        }

    }

    post {

        success {
            echo "Pipeline completado correctamente para ${env.APP_NAME}"
        }

        failure {
            echo "El pipeline falló."
        }

        always {
            echo "Build #${env.BUILD_NUMBER} finalizado."
        }

    }

}