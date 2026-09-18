pipeline {
    agent any

    environment {
        GITHUB_CREDS = credentials('ubuntu')
        JAVA_HOME    = tool name: 'jdk11'
        MAVEN_HOME   = tool name: 'maven3'
        PATH         = "${JAVA_HOME}/bin:${PATH}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Deploy') {
            steps {
                configFileProvider([configFile(fileId: '49f2a003-372b-4707-9389-8031e6140ee7', variable: 'MAVEN_SETTINGS')]) {
                    sh """
                        export GH_USER=${GITHUB_CREDS_USR}
                        export GH_TOKEN=${GITHUB_CREDS_PSW}

                        ${MAVEN_HOME}/bin/mvn -s $MAVEN_SETTINGS -B clean package
                        ${MAVEN_HOME}/bin/mvn -s $MAVEN_SETTINGS -B deploy
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and deployment to GitHub Packages completed successfully."
        }
        failure {
            echo "❌ Pipeline failed. Check the console output for details."
        }
    }
}
